---
title: Falhas de serviço de integridade
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 72b1593503db75aa275b9eb45c8342cee6724001
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280395"
---
# <a name="health-service-faults"></a>Falhas de serviço de integridade
> Aplica-se a: Windows Server 2019, Windows Server 2016

## <a name="what-are-faults"></a>Quais são as falhas

O serviço de integridade monitora constantemente o cluster de espaços de armazenamento diretos para detectar problemas e gerar "falhas". Um novo cmdlet exibe as falhas atuais, permitindo que você facilmente verificar a integridade da sua implantação sem examinar cada entidade ou recurso. As falhas são projetadas para serem precisas, fáceis de entender e acionáveis.  

Cada falha contém cinco campos importantes:  

-   Gravidade
-   Descrição do problema
-   Próximas etapas recomendadas para solucionar o problema
-   Informações de identificação para a entidade com falha
-   Sua localização física (se aplicável)

Por exemplo, aqui está uma falha típica:  

```
Severity: MINOR                                         
Reason: Connectivity has been lost to the physical disk.                           
Recommendation: Check that the physical disk is working and properly connected.    
Part: Manufacturer Contoso, Model XYZ9000, Serial 123456789                        
Location: Seattle DC, Rack B07, Node 4, Slot 11
```

 >[!NOTE]
 > A localização física é derivada da configuração do domínio de falha. Para obter mais informações sobre domínios de falha, consulte [domínios de falha no Windows Server 2016](fault-domains.md). Se você não fornecer essas informações, o campo local poderá ser menos útil (por exemplo, ele pode mostrar apenas o número de slot).  

## <a name="root-cause-analysis"></a>Análise da causa raiz

O serviço de integridade pode avaliar o potencial de causalidade entre entidades para identificar e combinar falhas que são consequências do mesmo problema subjacente a falha. Reconhecendo cadeias de efeito, isso gera relatórios menos longos. Por exemplo, se um servidor estiver inativo, espera-se que todas as unidades dentro do servidor também ficarão sem conectividade. Portanto, apenas a uma falha será gerada para a causa - nesse caso, o servidor.  

## <a name="usage-in-powershell"></a>Uso no PowerShell

Para ver as falhas atuais no PowerShell, execute este cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Isso retorna as falhas que afetam o cluster geral de espaços de armazenamento diretos. Geralmente, essas falhas estão relacionadas a hardware ou configuração. Se houver falha, este cmdlet não retornará nada.  

>[!NOTE]
> Em um ambiente de não produção e por seu próprio risco, você pode experimentar esse recurso disparando falhas por conta própria - por exemplo, removendo um disco físico ou desligando um nó. Depois que a falha foi exibido, insira novamente o disco físico ou reinicie que o nó e a falha desaparecerá novamente.

Você também pode exibir falhas que afetam somente determinados volumes ou compartilhamentos de arquivos com os seguintes cmdlets:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Isso retorna as falhas que afetam apenas o específico volume ou compartilhamento de arquivos. Geralmente, essas falhas estão relacionadas ao planejamento de capacidade, resiliência de dados ou recursos, como o armazenamento de qualidade de serviço ou réplica de armazenamento. 

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

### <a name="query-faults"></a>Falhas de consulta

Invocar **diagnosticar** para obter as falhas atuais no escopo para o destino **CimInstance**, que ser o cluster ou qualquer volume.

A lista completa de falhas disponíveis em cada escopo no Windows Server 2016 está documentada abaixo.

```       
public void GetFaults(CimSession Session, CimInstance Target)
{
    // Set Parameters (None)
    CimMethodParametersCollection FaultsParams = new CimMethodParametersCollection();
    // Invoke API
    CimMethodResult Result = Session.InvokeMethod(Target, "Diagnose", FaultsParams);
    IEnumerable<CimInstance> DiagnoseResults = (IEnumerable<CimInstance>)Result.OutParameters["DiagnoseResults"].Value;
    // Unpack
    if (DiagnoseResults != null)
    {
        foreach (CimInstance DiagnoseResult in DiagnoseResults)
        {
            // TODO: Whatever you want!
        }
    }
}
```

### <a name="optional-myfault-class"></a>Opcional: Classe MyFault

Talvez faça sentido para você construir e manter sua própria representação de falhas. Por exemplo, isso **MyFault** classe armazena várias propriedades importantes de falhas, incluindo o **FaultId**, que pode ser usado posteriormente para associar a atualização ou remover notificações de ou para eliminar a caso a mesma falha é detectada várias vezes, por algum motivo.

```       
public class MyFault {
    public String FaultId { get; set; }
    public String Reason { get; set; }
    public String Severity { get; set; }
    public String Description { get; set; }
    public String Location { get; set; }

    // Constructor
    public MyFault(CimInstance DiagnoseResult)
    {
        CimKeyedCollection<CimProperty> Properties = DiagnoseResult.CimInstanceProperties;
        FaultId     = Properties["FaultId"                  ].Value.ToString();
        Reason      = Properties["Reason"                   ].Value.ToString();
        Severity    = Properties["PerceivedSeverity"        ].Value.ToString();
        Description = Properties["FaultingObjectDescription"].Value.ToString();
        Location    = Properties["FaultingObjectLocation"   ].Value.ToString();
    }
}
```

```
List<MyFault> Faults = new List<MyFault>;

foreach (CimInstance DiagnoseResult in DiagnoseResults)
{
    Faults.Add(new Fault(DiagnoseResult));
}
```

A lista completa das propriedades em cada falha (**DiagnoseResult**) está documentada abaixo.

### <a name="fault-events"></a>Eventos de falha

Quando falhas são criadas, removidas ou atualizadas, o serviço de integridade gera eventos WMI. Eles são essenciais para manter o estado do seu aplicativo em sincronia sem sondagem frequente e podem ajudar com coisas como determinar quando enviar alertas por email, por exemplo. Para assinar esses eventos, esse código de exemplo usa o padrão de Design do observador novamente.

Primeiro, assine **MSFT\_StorageFaultEvent** eventos.

```      
public void ListenForFaultEvents()
{
    IObservable<CimSubscriptionResult> Events = Session.SubscribeAsync(
        @"root\microsoft\windows\storage", "WQL", "SELECT * FROM MSFT_StorageFaultEvent");
    // Subscribe the Observer
    FaultsObserver<CimSubscriptionResult> Observer = new FaultsObserver<CimSubscriptionResult>(this);
    IDisposable Disposeable = Events.Subscribe(Observer);
}   
```

Em seguida, implementar um observador cujos **OnNext()** método será invocado sempre que um novo evento é gerado.

Cada evento contém **ChangeType** que indica se uma falha está sendo criada, atualizado ou removido e o relevantes **FaultId**.

Além disso, eles contêm todas as propriedades de falha em si.

```
class FaultsObserver : IObserver
{
    public void OnNext(T Event)
    {
        // Cast
        CimSubscriptionResult SubscriptionResult = Event as CimSubscriptionResult;

        if (SubscriptionResult != null)
        {
            // Unpack            
            CimKeyedCollection<CimProperty> Properties = SubscriptionResult.Instance.CimInstanceProperties;
            String ChangeType = Properties["ChangeType"].Value.ToString();
            String FaultId = Properties["FaultId"].Value.ToString();

            // Create
            if (ChangeType == "0")
            {
                Fault MyNewFault = new MyFault(SubscriptionResult.Instance);
                // TODO: Whatever you want!
            }
            // Remove
            if (ChangeType == "1")
            {
                // TODO: Use FaultId to find and delete whatever representation you have...
            }
            // Update
            if (ChangeType == "2")
            {
                // TODO: Use FaultId to find and modify whatever representation you have...
            }
        }
    }
    public void OnError(Exception e)
    {
        // Handle Exceptions
    }
    public void OnCompleted()
    {
        // Nothing
    }
}
```

### <a name="understand-fault-lifecycle"></a>Entender o ciclo de vida de falha

Falhas não pretendem ser marcado como "Vista" ou resolvido pelo usuário. Eles são criados quando o serviço de integridade observa um problema, e eles serão removidos automaticamente e somente quando o serviço de integridade não pode observar o problema. Em geral, isso reflete que o problema foi corrigido.

No entanto, em alguns casos, falhas podem ser redescobertas pelo serviço de integridade (por exemplo, após o failover, ou devido à conectividade intermitente, etc.). Por esse motivo, talvez faz sentido para manter sua própria representação de falhas, portanto, você pode eliminar facilmente. Isso é especialmente importante se você enviar alertas por email ou equivalente.

### <a name="properties-of-faults"></a>Propriedades de falhas

Esta tabela apresenta várias propriedades importantes do objeto falha. Para o esquema completo, inspecione a **MSFT\_StorageDiagnoseResult** classe *storagewmi.mof*.

| **Propriedade**              | **Exemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Reason                    | "O volume está ficando sem espaço disponível."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Montar em rack A06, RU 25, Slot 11                                        |
| RecommendedActions        | {"Expandir o volume.", "Migrar cargas de trabalho para outros volumes."}   |

**FaultId** exclusiva dentro do escopo de um cluster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {"Informativo", "Aviso" e "Error"}, ou equivalentes cores como vermelho, amarelo e azul.

**FaultingObjectDescription** parte de informações de hardware, normalmente em branco para objetos de software.

**FaultingObjectLocation** informações de local de hardware, normalmente em branco para objetos de software.

**RecommendedActions** lista de ações recomendadas, que são independentes e sem nenhuma ordem específica. Atualmente, essa lista é muitas vezes de comprimento 1.

## <a name="properties-of-fault-events"></a>Propriedades de eventos de falha

Esta tabela apresenta várias propriedades importantes do evento de falha. Para o esquema completo, inspecione a **MSFT\_StorageFaultEvent** classe *storagewmi.mof*.

Observe a **ChangeType**, que indica se uma falha está sendo criada, removido ou atualizada e o **FaultId**. Um evento também contém todas as propriedades da falha de afetado.

| **Propriedade**              | **Exemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Reason                    | "O volume está ficando sem espaço disponível."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Montar em rack A06, RU 25, Slot 11                                        |
| RecommendedActions        | {"Expandir o volume.", "Migrar cargas de trabalho para outros volumes."}   |

**ChangeType** ChangeType = { 0, 1, 2 } = { "Create", "Remove", "Update" }.

## <a name="coverage"></a>Cobertura

No Windows Server 2016, o serviço de integridade fornece a seguinte cobertura de falha:  

### <a name="physicaldisk-8"></a>**PhysicalDisk (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* Gravidade: Aviso
* Motivo: *"O disco físico falhou."*
* RecommendedAction: *"Substituir o disco físico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* Gravidade: Aviso
* Motivo: *"Conectividade foi perdida para o disco físico."*
* RecommendedAction: *"Verificar que o disco físico está funcionando e conectados corretamente."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* Gravidade: Aviso
* Motivo: *"O disco físico está apresentando a falta de resposta recorrente".*
* RecommendedAction: *"Substituir o disco físico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* Gravidade: Aviso
* Motivo: *"Uma falha do disco físico está prevista para ocorrer em breve."*
* RecommendedAction: *"Substituir o disco físico."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* Gravidade: Aviso
* Motivo: *"O disco físico está em quarentena porque não é suportado pelo seu fornecedor de soluções."*
* RecommendedAction: *"Substituir o disco físico com o hardware com suporte".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* Gravidade: Aviso
* Motivo: *"O disco físico está em quarentena porque sua versão de firmware não é suportada pelo seu fornecedor de solução".*
* RecommendedAction: *"Atualizar o firmware no disco físico para a versão de destino".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* Gravidade: Aviso
* Motivo: *"O disco físico tem metadados não reconhecida".*
* RecommendedAction: *"Esse disco pode conter dados de um pool de armazenamento desconhecido. Primeiro verifique se que não há nenhum dado útil neste disco e o disco de redefinição."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* Gravidade: Aviso
* Motivo: *"Falha ao tentar atualizar o firmware no disco físico."*
* RecommendedAction: *"Tente usando um binário de firmware diferente."*

### <a name="virtual-disk-2"></a>**Disco virtual (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* Gravidade: Informativo
* Motivo: *"Alguns dados sobre este volume não estão totalmente resilientes. Permanece acessível."*
* RecommendedAction: *"Restauração de resiliência de dados".*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.Detached
* Gravidade: Crítico
* Motivo: *"O volume está inacessível. Alguns dados podem ser perdidos."*
* RecommendedAction: *"Verificar físico e/ou a conectividade de todos os dispositivos de armazenamento de rede. Você talvez precise restaurar do backup."*

### <a name="pool-capacity-1"></a>**Capacidade do pool (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType: Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* Gravidade: Aviso
* Motivo: *"O pool de armazenamento não tem a capacidade de reserva recomendada mínimo. Isso pode limitar sua capacidade de restaurar a resiliência de dados no caso de falha de unidade (s)."*
* RecommendedAction: *"Adicionar capacidade adicional para o pool de armazenamento ou libere a capacidade. O mínimo recomendado reserva varia de acordo com a implantação, mas é a importância dos aproximadamente 2 unidades de capacidade."*

### <a name="volume-capacity-2sup1sup"></a>**A capacidade de volume (2)** <sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Gravidade: Aviso
* Motivo: *"O volume está ficando sem espaço disponível."*
* RecommendedAction: *"Expandir o volume ou migrar cargas de trabalho para outros volumes."*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Gravidade: Crítico
* Motivo: *"O volume está ficando sem espaço disponível."*
* RecommendedAction: *"Expandir o volume ou migrar cargas de trabalho para outros volumes."*

### <a name="server-3"></a>**Servidor (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType: Microsoft.Health.FaultType.Server.Down
* Gravidade: Crítico
* Motivo: *"O servidor não pode ser alcançado."*
* RecommendedAction: *"Iniciar ou substituir o servidor".*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType: Microsoft.Health.FaultType.Server.Isolated
* Gravidade: Crítico
* Motivo: *"O servidor está isolado do cluster devido a problemas de conectividade."*
* RecommendedAction: *"Se isolamento persistir, verifique as redes ou migrar cargas de trabalho para outros nós."*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType: Microsoft.Health.FaultType.Server.Quarantined
* Gravidade: Crítico
* Motivo: *"O servidor está em quarentena pelo cluster devido a falhas recorrentes."*
* RecommendedAction: *"Substituir o servidor ou corrigir a rede".*

### <a name="cluster-1"></a>**Cluster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType: Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* Gravidade: Crítico
* Motivo: *"O cluster é uma falha do servidor para longe de ficar inativo".*
* RecommendedAction: *"Verifique o recurso de testemunha e reinicie conforme necessário. Iniciar ou substituir servidores com falha".*

### <a name="network-adapterinterface-4"></a>**Adaptador de rede/Interface (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* Gravidade: Aviso
* Motivo: *"O adaptador de rede tornam-se desconectou."*
* RecommendedAction: *"Se reconectar o cabo de rede."*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType: Microsoft.Health.FaultType.NetworkInterface.Missing
* Gravidade: Aviso
* Motivo: *"O servidor {server} tem ausente conectado à rede de cluster {rede de cluster} de adaptadores de rede."*
* RecommendedAction: *"Conectar o servidor à rede de cluster ausente".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Hardware
* Gravidade: Aviso
* Motivo: *"O adaptador de rede teve uma falha de hardware".*
* RecommendedAction: *"Substituir o adaptador de interface de rede".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disabled
* Gravidade: Aviso
* Motivo: *"O adaptador de rede {interface de rede} não está habilitado e não está sendo usado."*
* RecommendedAction: *"Habilitar a interface de rede".*

### <a name="enclosure-6"></a>**Compartimento (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* Gravidade: Aviso
* Motivo: *"Comunicação foi perdida para o compartimento de armazenamento."*
* RecommendedAction: *"Iniciar ou substituir o compartimento de armazenamento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.FanError
* Gravidade: Aviso
* Motivo: *"O ventilador na posição {posição} do compartimento de armazenamento falhou."*
* RecommendedAction: *"Substituir o ventilador no compartimento de armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* Gravidade: Aviso
* Motivo: *"O sensor atual na posição {posição} do compartimento de armazenamento falhou."*
* RecommendedAction: *"Substituir um sensor de corrente no compartimento de armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* Gravidade: Aviso
* Motivo: *"O sensor de tensão na posição {posição} do compartimento de armazenamento falhou."*
* RecommendedAction: *"Substituir um sensor de tensão no compartimento de armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* Gravidade: Aviso
* Motivo: *"O controlador de e/s na posição {posição} do compartimento de armazenamento falhou."*
* RecommendedAction: *"Substituir um controlador de e/s no compartimento de armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* Gravidade: Aviso
* Motivo: *"O sensor de temperatura na posição {posição} do compartimento de armazenamento falhou."*
* RecommendedAction: *"Substituir um sensor de temperatura no compartimento de armazenamento".*

### <a name="firmware-rollout-3"></a>**Distribuição de firmware (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* Gravidade: Aviso
* Motivo: *"No momento, não é possível tornar o progresso durante a execução de firmware de lançamento."*
* RecommendedAction: *"Verifique se todos os espaços de armazenamento estão íntegros e que nenhum domínio de falha está atualmente no modo de manutenção."*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* Gravidade: Aviso
* Motivo: *"O firmware de lançamento foi cancelado devido a informações de versão de firmware não pode ser lido ou inesperado após a aplicação de uma atualização de firmware."*
* RecommendedAction: *"Reinicialização firmware distribuir depois que o firmware foi resolvido."*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* Gravidade: Aviso
* Motivo: *"O firmware de lançamento foi cancelado devido a muitos discos físicos com falha de uma tentativa de atualização de firmware."*
* RecommendedAction: *"Reinicialização firmware distribuir depois que o firmware foi resolvido."*

### <a name="storage-qos-3sup2sup"></a>**QoS de armazenamento (3)** <sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType: Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* Gravidade: Aviso
* Motivo: *"Taxa de transferência de armazenamento é insuficiente para satisfazer as reservas de".*
* RecommendedAction: *"Reconfigurar as políticas de QoS de armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorQos.LostCommunication
* Gravidade: Aviso
* Motivo: *"O Gerenciador de políticas de QoS de armazenamento perdeu a comunicação com o volume."*
* RecommendedAction: *"Reinicialize nós {nós}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType: Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* Gravidade: Aviso
* Motivo: *"Um ou mais consumidores de armazenamento (normalmente, máquinas virtuais) estão usando uma política não existente com a id {id}".*
* RecommendedAction: *"Recriar as políticas de QoS de armazenamento ausentes".*

<sup>1</sup> indica que o volume tiver atingido a 80% cheio (severidade menor) ou 90% cheio (severidade principal).  
<sup>2</sup> indica que alguns .vhd(s) no volume não atendidos seu IOPS mínimo para acima de 10% (secundário), 30% (principal) ou 50% (crítico) de reverter a janela de 24 horas.  

>[!NOTE]
> A integridade dos componentes do compartimento de armazenamento, como sensores, fontes de alimentação e ventiladores, é derivada de SES (Serviços de Compartimento) SCSI. Se o fornecedor não lhe der essas informações, o Serviço de Integridade não poderá exibi-las.  

## <a name="see-also"></a>Consulte também

- [Serviço de integridade no Windows Server 2016](health-service-overview.md)
