---
title: "Relatórios de serviço de integridade"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: bc21b9fdec5700fec23dc6af7ca15873ded34bea
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-reports"></a>Relatórios de serviço de integridade
> Aplica-se para o Windows Server 2016

## <a name="what-are-reports"></a>Quais são relatórios?  

O serviço de integridade reduz o trabalho necessário para obter desempenho ao vivo e informações de capacidade de cluster direta de espaços de armazenamento. Um novo cmdlet fornece uma lista administrada métricas essenciais, que são coletados com eficiência e agregados dinamicamente em todos os nós, com lógica interna para detectar a associação ao grupo de cluster. Todos os valores são apenas no momento e em tempo real.  

## <a name="usage-in-powershell"></a>Uso do PowerShell

Use este cmdlet para obter métricas de todo o cluster de espaços de armazenamento direto:

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport
```

Opcional **contagem** parâmetro indica quantos conjuntos de valores para retornar, intervalos de um segundo.  

```PowerShell
Get-StorageSubSystem Cluster* | Get-StorageHealthReport -Count <Count>  
```

Você também pode obter métricas para um volume específico ou servidor:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Get-StorageHealthReport -Count <Count>  

Get-StorageNode -Name <Name> | Get-StorageHealthReport -Count <Count>
```

## <a name="usage-in-net-and-c"></a>Uso em .NET e c#

### <a name="connect"></a>Conectar-se

Para consultar o serviço de integridade, você precisará estabelecer uma **CimSession** com o cluster. Para fazer isso, você precisará de algumas coisas que só estão disponíveis no .NET completo, ou seja, você não pode prontamente fazer isso diretamente de um web ou aplicativo móvel. Esses exemplos de código usará c \ #, a opção mais simples para essa camada de acesso de dados.

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

O nome do usuário fornecido deve ser um administrador local do computador de destino.

É recomendável que você constrói a senha **SecureString** diretamente da entrada do usuário em tempo real, então suas senhas jamais seja armazenada na memória em texto sem formatação. Isso ajuda a minimizar uma variedade de questões de segurança. Mas, na prática, é comum para fins de criação de protótipos construí-la como explicado acima.

### <a name="discover-objects"></a>Descobrir objetos

Com o **CimSession** estabelecida, você pode consultar Windows Management Instrumentation (WMI) no cluster.

Antes de poder falhas ou métricas, você precisará obter instâncias de vários objetos relevantes. Primeiro, o **MSFT\_StorageSubSystem** que representa direta de espaços de armazenamento no cluster. Fazer isso, você pode obter cada **MSFT\_StorageNode** no cluster e cada **MSFT\_Volume**, os volumes de dados. Por fim, você precisará do **MSFT\_StorageHealth**, a integridade do serviço em si, também.

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

Estes são os mesmos objetos que você obtenha no PowerShell usando os cmdlets como **Get-StorageSubSystem**, **Get-StorageNode**, e **Volume Get**.

Você pode acessar todos os mesmos propriedades, documentadas em [Classes de API de gerenciamento de armazenamento](https://msdn.microsoft.com/en-us/library/windows/desktop/hh830612(v=vs.85).aspx).

```
...
using System.Diagnostics;

foreach (CimInstance Node in Nodes)
{
    // For illustration, write each node's Name to the console. You could also write State (up/down), or anything else!
    Debug.WriteLine("Discovered Node " + Node.CimInstanceProperties["Name"].Value.ToString());
}
```

Invocar **GetReport** começar o streaming amostras de uma lista de comissionada especialista métricas essenciais, que são coletados com eficiência e agregados dinamicamente em todos os nós, com lógica interna para detectar a associação ao grupo de cluster. Amostras chegarão a cada segundo daí em diante. Todos os valores são apenas no momento e em tempo real.

Métricas podem ser transmitidas para três escopos: cluster, qualquer nó ou qualquer volume.

A lista completa de métricas disponíveis em cada escopo no Windows Server 2016 é documentada abaixo.

### <a name="iobserveronnext"></a>IObserver.OnNext()

Este código de exemplo usa o [padrão de Design do observador](https://msdn.microsoft.com/en-us/library/ee850490(v=vs.110).aspx) implementar um observador cujo **OnNext()** método será invocado quando chega de cada novo exemplo de métricas. Sua **OnCompleted()** método será chamado se/quando terminar de streaming. Por exemplo, você pode usá-lo para reiniciar o streaming, portanto, ele continua indefinidamente.

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

### <a name="begin-streaming"></a>Comece a transmissão

Com o observador definido, você pode começar streaming.

Especificar o destino **CimInstance** a serem as métricas no escopo. Ele pode ser o cluster, qualquer nó ou qualquer volume.

O parâmetro count é o número de amostras antes de streaming fim.

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

Obviamente, essas métricas podem ser visualizadas, armazenada em um banco de dados, ou usados da forma como desejar.

### <a name="properties-of-reports"></a>Propriedades de relatórios

Cada amostra de métricas é um "relatório" que contém muitos "registros" correspondente às métricas individuais.

Para o esquema completo, inspecionar o **MSFT\_StorageHealthReport** e **MSFT\_HealthRecord** as classes no *storagewmi.mof*.

Cada métrica tem apenas três propriedades, por esta tabela.

| **Propriedade** | **Exemplo**       |
| -------------|-------------------|
| Nome         | IOLatencyAverage  |
| Valor        | 0.00021           |
| Unidades        | 3                 |

Unidades = {0, 1, 2, 3 e 4}, onde 0 = "Bytes", 1 = "BytesPerSecond", 2 = "CountPerSecond", 3 = "Segundos" ou 4 = "Porcentagem".

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
