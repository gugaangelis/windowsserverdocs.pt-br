---
title: Relatórios de integridade do serviço
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: e018c0270a0bf410dada9c05d2c25e51fdfac1d8
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280162"
---
# <a name="health-service-reports"></a>Relatórios de integridade do serviço
> Aplica-se a: Windows Server 2019, Windows Server 2016

## <a name="what-are-reports"></a>Quais são os relatórios  

O serviço de integridade reduz o trabalho necessário para obter informações de capacidade e desempenho em tempo real do seu cluster de espaços de armazenamento diretos. Um novo cmdlet fornece uma lista estruturada de métricas essenciais, que são coletadas com eficiência e agregadas dinamicamente em nós, com lógica interna para detectar a associação do cluster. Todos os valores são apenas pontuais e em tempo real.  

## <a name="usage-in-powershell"></a>Uso no PowerShell

Use este cmdlet para obter métricas de todo o cluster de espaços de armazenamento diretos:

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

Opcional **contagem** parâmetro indica quantos conjuntos de valores devem ser retornados em intervalos de um segundo.  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

Você também pode obter as métricas para um volume específico ou servidor:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

## <a name="usage-in-net-and-c"></a>Uso no .NET eC#

### <a name="connect"></a>Conectar

Para consultar o serviço de integridade, você precisa estabelecer um **CimSession** com o cluster. Para fazer isso, você precisará de algumas coisas que só estão disponíveis no .NET completo, isso significa que você não pode prontamente fazer isso diretamente de um aplicativo web ou móvel. Esses exemplos de código usará C\#, mais simples choice para esta camada de acesso de dados.

``` 
...
using System.Security;
using Microsoft.Management.Infrastructure;

public CimSession Connect(string Domain = "...", string Computer = "...", string Username = "...", string Password = "...")
{
    SecureString PasswordSecureString = new SecureString();
    foreach (char c in Password)
    {
        PasswordSecureString.AppendChar(c);
    }

    CimCredential Credentials = new CimCredential(
        PasswordAuthenticationMechanism.Default, Domain, Username, PasswordSecureString);
    WSManSessionOptions SessionOptions = new WSManSessionOptions();
    SessionOptions.AddDestinationCredentials(Credentials);
    Session = CimSession.Create(Computer, SessionOptions);
    return Session;
}
```

O nome de usuário fornecido deve ser um administrador local do computador de destino.

É recomendável que você construa a senha **SecureString** diretamente da entrada do usuário em tempo real, portanto, sua senha nunca seja armazenada na memória em texto não criptografado. Isso ajuda a atenuar uma variedade de questões de segurança. Mas, na prática, é comum para fins de criação de protótipos de construí-la como acima.

### <a name="discover-objects"></a>Descobrir objetos

Com o **CimSession** estabelecida, você pode consultar Windows Management Instrumentation (WMI) no cluster.

Antes de poder obter falhas ou métricas, você precisará obter instâncias de vários objetos relevantes. Primeiro, o **MSFT\_StorageSubSystem** que representa espaços de armazenamento diretos no cluster. Usando essa, você pode obter cada **MSFT\_StorageNode** no cluster e cada **MSFT\_Volume**, os volumes de dados. Por fim, você precisará de **MSFT\_StorageHealth**, o serviço de integridade em si, muito.

```
CimInstance Cluster;
List<CimInstance> Nodes;
List<CimInstance> Volumes;
CimInstance HealthService;

public void DiscoverObjects(CimSession Session)
{
    // Get MSFT_StorageSubSystem for Storage Spaces Direct
    Cluster = Session.QueryInstances(@"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageSubSystem")
        .First(Instance => (Instance.CimInstanceProperties["FriendlyName"].Value.ToString()).Contains("Cluster"));

    // Get MSFT_StorageNode for each cluster node
    Nodes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageNode", null, "StorageSubSystem", "StorageNode").ToList();

    // Get MSFT_Volumes for each data volume
    Volumes = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToVolume", null, "StorageSubSystem", "Volume").ToList();

    // Get MSFT_StorageHealth itself
    HealthService = Session.EnumerateAssociatedInstances(Cluster.CimSystemProperties.Namespace,
        Cluster, "MSFT_StorageSubSystemToStorageHealth", null, "StorageSubSystem", "StorageHealth").First();
}
```

Esses são os mesmos objetos de obter no PowerShell usando os cmdlets como **Get-StorageSubSystem**, **Get-StorageNode**, e **Get-Volume**.

Você pode acessar as mesmas propriedades, documentadas em [Classes de API de gerenciamento de armazenamento](https://msdn.microsoft.com/library/windows/desktop/hh830612(v=vs.85).aspx).

```
...
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

Invocar **GetReport** para iniciar o streaming de exemplos de uma lista de curadoria de especialista de métricas essenciais, que são coletadas com eficiência e agregadas dinamicamente em nós, com lógica interna para detectar a associação do cluster. Exemplos de chegará a cada segundo depois disso. Todos os valores são apenas pontuais e em tempo real.

As métricas podem ser transmitidas para os três escopos: o cluster, qualquer nó ou qualquer volume.

A lista completa de métricas disponíveis em cada escopo no Windows Server 2016 está documentada abaixo.

### <a name="iobserveronnext"></a>IObserver.OnNext()

Esse código de exemplo usa o [padrão de Design do observador](https://msdn.microsoft.com/library/ee850490(v=vs.110).aspx) para implementar um observador cujo **OnNext()** método será invocado quando cada novo exemplo de métricas é recebido. Sua **OnCompleted()** método será chamado se/quando termina de streaming. Por exemplo, você pode usá-lo para reiniciar a transmissão, portanto, ele continua indefinidamente.

```
class MetricsObserver<T> : IObserver<T>
{
    public void OnNext(T Result)
    {
        // Cast
        CimMethodStreamedResult StreamedResult = Result as CimMethodStreamedResult;

        if (StreamedResult != null)
        {
            // For illustration, you could store the metrics in this dictionary
            Dictionary<string, string> Metrics = new Dictionary<string, string>();

            // Unpack
            CimInstance Report = (CimInstance)StreamedResult.ItemValue;
            IEnumerable<CimInstance> Records = (IEnumerable<CimInstance>)Report.CimInstanceProperties["Records"].Value;
            foreach (CimInstance Record in Records)
            {
                /// Each Record has "Name", "Value", and "Units"
                Metrics.Add(
                    Record.CimInstanceProperties["Name"].Value.ToString(),
                    Record.CimInstanceProperties["Value"].Value.ToString()
                    );
            }

            // TODO: Whatever you want!
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Reinvoke BeginStreamingMetrics(), defined in the next section
    }
}
```

### <a name="begin-streaming"></a>Iniciar o streaming

Com o observador definido, você pode iniciar o streaming.

Especifique o destino **CimInstance** ao qual você deseja que as métricas no escopo. Ele pode ser qualquer volume, qualquer nó ou o cluster.

O parâmetro de contagem é o número de amostras antes de extremidades de streaming.

```
CimInstance Target = Cluster; // From among the objects discovered in DiscoverObjects()

public void BeginStreamingMetrics(CimSession Session, CimInstance HealthService, CimInstance Target)
{
    // Set Parameters
    CimMethodParametersCollection MetricsParams = new CimMethodParametersCollection();
    MetricsParams.Add(CimMethodParameter.Create("TargetObject", Target, CimType.Instance, CimFlags.In));
    MetricsParams.Add(CimMethodParameter.Create("Count", 999, CimType.UInt32, CimFlags.In));
    // Enable WMI Streaming
    CimOperationOptions Options = new CimOperationOptions();
    Options.EnableMethodResultStreaming = true;
    // Invoke API
    CimAsyncMultipleResults<CimMethodResultBase> InvokeHandler;
    InvokeHandler = Session.InvokeMethodAsync(
        HealthService.CimSystemProperties.Namespace, HealthService, "GetReport", MetricsParams, Options
        );
    // Subscribe the Observer
    MetricsObserver<CimMethodResultBase> Observer = new MetricsObserver<CimMethodResultBase>(this);
    IDisposable Disposeable = InvokeHandler.Subscribe(Observer);
}
```

Obviamente, essas métricas podem ser visualizadas, armazenado em um banco de dados ou que foi usado na maneira que desejar.

### <a name="properties-of-reports"></a>Propriedades de relatórios

Cada exemplo de métricas é um "relatório" que contém muitos "registros" correspondente ao métricas individuais.

Para o esquema completo, inspecione a **MSFT\_StorageHealthReport** e **MSFT\_HealthRecord** as classes *storagewmi.mof*.

Cada métrica tem apenas três propriedades, por essa tabela.

| **Propriedade** | **Exemplo**       |
| -------------|-------------------|
| Nome         | IOLatencyAverage  |
| Valor        | 0.00021           |
| Unidades        | 3                 |

Unidades = {0, 1, 2, 3, 4}, onde 0 = "Bytes", 1 = "BytesPerSecond", 2 = "CountPerSecond", 3 = "Segundos", ou 4 = "Porcentagem".

## <a name="coverage"></a>Cobertura

Abaixo estão as métricas disponíveis para cada escopo no Windows Server 2016.

### <a name="msftstoragesubsystem"></a>MSFT_StorageSubSystem

| **Nome**                        | **Unidades** |
|---------------------------------|-----------|
| CPUUsage                        | 4         |
| CapacityPhysicalPooledAvailable | 0         |
| CapacityPhysicalPooledTotal     | 0         |
| CapacityPhysicalTotal           | 0         |
| CapacityPhysicalUnpooled        | 0         |
| CapacityVolumesAvailable        | 0         |
| CapacityVolumesTotal            | 0         |
| IOLatencyAverage                | 3         |
| IOLatencyRead                   | 3         |
| IOLatencyWrite                  | 3         |
| IOPSRead                        | 2         |
| IOPSTotal                       | 2         |
| IOPSWrite                       | 2         |
| IOThroughputRead                | 1         |
| IOThroughputTotal               | 1         |
| IOThroughputWrite               | 1         |
| MemoryAvailable                 | 0         |
| MemoryTotal                     | 0         |


### <a name="msftstoragenode"></a>MSFT_StorageNode

| **Nome**            | **Unidades** |
|---------------------|-----------|
| CPUUsage            | 4         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |
| MemoryAvailable     | 0         |
| MemoryTotal         | 0         |

### <a name="msftvolume"></a>MSFT_Volume

| **Nome**            | **Unidades** |
|---------------------|-----------|
| CapacityAvailable   | 0         |
| CapacityTotal       | 0         |
| IOLatencyAverage    | 3         |
| IOLatencyRead       | 3         |
| IOLatencyWrite      | 3         |
| IOPSRead            | 2         |
| IOPSTotal           | 2         |
| IOPSWrite           | 2         |
| IOThroughputRead    | 1         |
| IOThroughputTotal   | 1         |
| IOThroughputWrite   | 1         |

## <a name="see-also"></a>Consulte também

- [Serviço de integridade no Windows Server 2016](health-service-overview.md)
