---
title: "Falhas de serviço de integridade"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: c9e1fb4568ee93739c49ccc1a13106b09161c5f3
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-faults"></a>Falhas de serviço de integridade
> Aplica-se para o Windows Server 2016

## <a name="what-are-faults"></a>Quais são falhas?

O serviço de integridade constantemente monitora o cluster direta de espaços de armazenamento para detectar problemas e gerar "falhas". Um novo cmdlet exibe quaisquer falhas atuais, permitindo que você facilmente verificar a integridade da implantação sem olhar cada entidade ou por sua vez de recursos. Falhas são projetadas para ser precisos, fácil de entender e acionáveis.  

Cada falha contém cinco campos importantes:  

-   Severity
-   Descrição do problema
-   Recomendado próximas etapas para resolver o problema
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
 > A localização física é derivada de sua configuração de domínio falha. Para obter mais informações sobre domínios de falha, consulte [domínios de falha no Windows Server 2016](fault-domains.md). Se você não fornecer essas informações, o campo do local será menos úteis - por exemplo, ele pode mostrar apenas o número do slot.  

## <a name="root-cause-analysis"></a>Análise de causas

O serviço de integridade pode avaliar a possível casualidade entre com falha entidades para identificar e combinar falhas que são consequências do mesmo problema subjacente. Reconhecendo cadeias de efeito, isso torna por menos relatar tagarelas. Por exemplo, se um servidor for pressionada, espera-se de quaisquer unidades dentro do servidor também será sem conectividade. Portanto, apenas uma falha será gerada para a causa - nesse caso, o servidor.  

## <a name="usage-in-powershell"></a>Uso do PowerShell

Para ver qualquer falha atual no PowerShell, execute este cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Isso retorna quaisquer falhas que afetam o cluster de espaços de armazenamento direta geral. Geralmente, essas falhas relacionam a hardware ou configuração. Se não houver nenhuma falha, este cmdlet não retornará nada.  

>[!NOTE]
> Em um ambiente de não produção e de sua responsabilidade, você pode experimentar esse recurso por disparando a falhas - por exemplo, removendo um disco físico ou desligar um nó. Quando aparece a falha, insira novamente o disco físico ou reiniciar que o nó e a falha desaparecerá novamente.

Você também pode ver falhas que afetam apenas volumes específicos ou compartilhamentos de arquivos com os seguintes cmdlets:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Isso retorna quaisquer falhas que afetam apenas o compartilhamento volume ou arquivo específico. Geralmente, essas falhas se relacionam com planejamento da capacidade, resiliência de dados ou recursos, como qualidade de serviço de armazenamento ou réplica de armazenamento. 

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

### <a name="query-faults"></a>Falhas de consulta

Invocar **diagnosticar** para obter quaisquer falhas atuais no escopo para o destino **CimInstance**, que seja o cluster ou qualquer volume.

A lista completa de falhas disponíveis em cada escopo no Windows Server 2016 é documentada abaixo.

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

### <a name="optional-myfault-class"></a>Opcional: MyFault classe

Ele pode fazer sentido para construir e manter seu próprios representação de falhas. Por exemplo, isso **MyFault** classe armazena várias propriedades de chave de falhas, incluindo o **FaultId**, que pode ser usado posteriormente para associar a atualização ou remover notificações, ou para elimine que é a mesma falha detectadas várias vezes, por qualquer motivo.

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

A lista completa de propriedades em cada falha (**DiagnoseResult**) estão documentadas abaixo.

### <a name="fault-events"></a>Eventos de falha

Quando falhas são criadas, removidas ou atualizadas, o serviço de integridade gera eventos de WMI. Esses são essenciais para manter o estado do aplicativo em sincronia sem sondagem frequente e podem ajudar a coisas como determinar quando enviar alertas de email, por exemplo. Para assinar esses eventos, esse código de exemplo usa o padrão de Design do observador novamente.

Primeiro, assinar **MSFT\_StorageFaultEvent** eventos.

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

Em seguida, implemente um observador cujo **OnNext()** método será invocado sempre que um novo evento é gerado.

Cada evento contém **ChangeType** indicando se uma falha está sendo criada, removido ou atualizado e relevante **FaultId**.

Além disso, eles contêm todas as propriedades da falha em si.

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

Falhas não se destinam a ser marcado como "visto" ou resolvido pelo usuário. Eles são criados quando o serviço de integridade observa um problema, e eles são removidos automaticamente e somente quando o serviço de integridade não pode observar o problema. Em geral, isso indica que o problema foi corrigido.

No entanto, em alguns casos, falhas podem ser redescobertas pelo serviço de integridade (por exemplo, depois de failover ou devido à conectividade intermitente, etc.). Por esse motivo, talvez faz sentido para manter sua própria representação de falhas, portanto, você pode facilmente elimine. Isso é especialmente importante se você enviar alertas de email ou equivalente.

### <a name="properties-of-faults"></a>Propriedades de falhas

Esta tabela apresenta várias propriedades de chave do objeto falha. Para o esquema completo, inspecionar o **MSFT\_StorageDiagnoseResult** de classe no *storagewmi.mof*.

| **Propriedade**              | **Exemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Motivo                    | "O volume estiver ficando sem espaço disponível".                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Coletar A06, RU 25, Slot de 11                                        |
| RecommendedActions        | {"Expandir o volume.", "Migrar cargas de trabalho para outros volumes."}   |

**FaultId** exclusivo no escopo de um cluster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {"Informativo", "Aviso" e "Erro"}, ou equivalente cores, como vermelho, azul e amarelo.

**FaultingObjectDescription** parte informações de hardware, normalmente em branco para objetos de software.

**FaultingObjectLocation** informações de localização para hardware, normalmente em branco para objetos de software.

**RecommendedActions** lista de ações recomendadas, que são independentes e sem uma ordem específica. Hoje, essa lista é geralmente de comprimento 1.

## <a name="properties-of-fault-events"></a>Propriedades de eventos de falha

Esta tabela apresenta várias propriedades de chaves do evento falha. Para o esquema completo, inspecionar o **MSFT\_StorageFaultEvent** de classe no *storagewmi.mof*.

Observação o **ChangeType**, que indica se uma falha é sendo criada, removidos ou atualizados e o **FaultId**. Um evento também contém todas as propriedades da falha de afetados.

| **Propriedade**              | **Exemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| FaultId                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft.Health.FaultType.Volume.Capacity                      |
| Motivo                    | "O volume estiver ficando sem espaço disponível".                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Coletar A06, RU 25, Slot de 11                                        |
| RecommendedActions        | {"Expandir o volume.", "Migrar cargas de trabalho para outros volumes."}   |

**ChangeType** ChangeType = {0, 1, 2} = {"criar", "Remover", "Atualizar"}.

## <a name="coverage"></a>Cobertura

No Windows Server 2016, o serviço de integridade fornece a cobertura de falha a seguir:  

### **<a name="physicaldisk-8"></a>Disco físico (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedMedia
* Severity: aviso
* Motivo: *"no disco físico falhou."*
* RecommendedAction: *"Substituir o disco físico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.LostCommunication
* Severity: aviso
* Motivo: *"Conectividade foi perdida no disco físico."*
* RecommendedAction: *"Check-se de que o disco físico está conectado corretamente e funcionando."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.Unresponsive
* Severity: aviso
* Motivo: *"o disco físico está apresentando falta de resposta recorrente."*
* RecommendedAction: *"Substituir o disco físico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.PredictiveFailure
* Severity: aviso
* Motivo: *"uma falha do disco físico é prevista para ocorrer em breve."*
* RecommendedAction: *"Substituir o disco físico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedHardware
* Severity: aviso
* Motivo: *"o disco físico é colocado em quarentena porque ele não é compatível com o fornecedor da solução."*
* RecommendedAction: *"Substituir o disco físico hardware com suporte".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnsupportedFirmware
* Severity: aviso
* Motivo: *"o disco físico é em quarentena porque sua versão do firmware não é compatível com o fornecedor da solução."*
* RecommendedAction: *"Atualize o firmware no disco físico para a versão de destino".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.UnrecognizedMetadata
* Severity: aviso
* Motivo: *"o disco físico tem não reconhecido metadados."*
* RecommendedAction: *"esse disco pode conter dados de um pool de armazenamento desconhecido. Primeiro verifique se que não há nenhum dado útil no disco e o disco de redefinição."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType: Microsoft.Health.FaultType.PhysicalDisk.FailedFirmwareUpdate
* Severity: aviso
* Motivo: *"Falha na tentativa de atualizar o firmware no disco físico".*
* RecommendedAction: *"Tente usar um binário de firmware diferente."*

### **<a name="virtual-disk-2"></a>Disco virtual (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.NeedsRepair
* Severity: informativa
* Motivo: *"alguns dados neste volume não são totalmente flexíveis. Permanece acessível."*
* RecommendedAction: *"Restaurando a resiliência dos dados".*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType: Microsoft.Health.FaultType.VirtualDisks.Detached
* Severity: críticas
* Motivo: *"o volume não está acessível. Alguns dados poderão ser perdidos."*
* RecommendedAction: *"Verificar físico e/ou conectividade de todos os dispositivos de armazenamento de rede. Você precisará restaurar a partir do backup."*

### **<a name="pool-capacity-1"></a>Capacidade do pool (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType: Microsoft.Health.FaultType.StoragePool.InsufficientReserveCapacityFault
* Severity: aviso
* Motivo: *"pool de armazenamento não tem a capacidade de reserva recomendada mínima. Isso pode limitar sua capacidade de restaurar resiliência de dados no caso de falhas de unidade."*
* RecommendedAction: *"adicionar a capacidade adicional ao pool de armazenamento ou liberar capacidade. O mínimo recomendado reservar varia de acordo com a implantação, mas é o valor dos aproximadamente 2 unidades de capacidade."*

### <a name="volume-capacity-2sup1sup"></a>**A capacidade de volume (2)**<sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Severity: aviso
* Motivo: *"o volume estiver ficando sem espaço disponível".*
* RecommendedAction: *"Expandir o volume ou migrar cargas de trabalho para outros volumes."*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft.Health.FaultType.Volume.Capacity
* Severity: críticas
* Motivo: *"o volume estiver ficando sem espaço disponível".*
* RecommendedAction: *"Expandir o volume ou migrar cargas de trabalho para outros volumes."*

### **<a name="server-3"></a>Servidor (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType: Microsoft.Health.FaultType.Server.Down
* Severity: críticas
* Motivo: *"o servidor não pode ser acessado."*
* RecommendedAction: *"Iniciar ou substituir o servidor."*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType: Microsoft.Health.FaultType.Server.Isolated
* Severity: críticas
* Motivo: *"o servidor é isolado do cluster devido a problemas de conectividade."*
* RecommendedAction: *"Se o isolamento de persistir, verifique as redes ou migrar cargas de trabalho para outros nós."*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType: Microsoft.Health.FaultType.Server.Quarantined
* Severity: críticas
* Motivo: *"o servidor está em quarentena pelo cluster devido a falhas recorrentes."*
* RecommendedAction: *"Substituir o servidor ou corrigir a rede".*

### **<a name="cluster-1"></a>Cluster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType: Microsoft.Health.FaultType.ClusterQuorumWitness.Error
* Severity: críticas
* Motivo: *"cluster é uma falha de servidor longe diminuindo."*
* RecommendedAction: *"verificar se o recurso de testemunha e reiniciar conforme necessário. Iniciar ou substituir servidores que falharam."*

### **<a name="network-adapterinterface-4"></a>Adaptador de rede/Interface (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disconnected
* Severity: aviso
* Motivo: *"a interface de rede foi desconectada."*
* RecommendedAction: *"Reconectar o cabo de rede".*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType: Microsoft.Health.FaultType.NetworkInterface.Missing
* Severity: aviso
* Motivo: *"o servidor {servidor} tem ausentes conectados à rede de cluster {cluster} de adaptadores de rede."*
* RecommendedAction: *"Conectar o servidor na rede ausentes do cluster".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Hardware
* Severity: aviso
* Motivo: *"a interface de rede teve uma falha de hardware."*
* RecommendedAction: *"Substituir o adaptador de interface de rede".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType: Microsoft.Health.FaultType.NetworkAdapter.Disabled
* Severity: aviso
* Motivo: *"{NetworkInterface} de interface de rede não está habilitada e não está sendo usada."*
* RecommendedAction: *"habilitar a interface de rede."*

### **<a name="enclosure-6"></a>Compartimento (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.LostCommunication
* Severity: aviso
* Motivo: *"Comunicação foi perdida ao compartimento de armazenamento."*
* RecommendedAction: *"Iniciar ou substituir o compartimento de armazenamento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.FanError
* Severity: aviso
* Motivo: *"na posição {posição} do compartimento armazenamento houve falha no ventilador."*
* RecommendedAction: *"Substituir o leque no compartimento da armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.CurrentSensorError
* Severity: aviso
* Motivo: *"o sensor atual na posição {posição} do compartimento armazenamento falhou."*
* RecommendedAction: *"Substituir um sensor atual no compartimento da armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.VoltageSensorError
* Severity: aviso
* Motivo: *"o sensor de tensão na posição {posição} do compartimento armazenamento falhou."*
* RecommendedAction: *"Substituir um sensor de tensão no compartimento da armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.IoControllerError
* Severity: aviso
* Motivo: *"o controlador de e/s na posição {posição} do compartimento armazenamento falhou."*
* RecommendedAction: *"Substituir um controlador de e/s no compartimento da armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType: Microsoft.Health.FaultType.StorageEnclosure.TemperatureSensorError
* Severity: aviso
* Motivo: *"o sensor de temperatura na posição {posição} do compartimento armazenamento falhou."*
* RecommendedAction: *"Substituir um sensor de temperatura no compartimento da armazenamento".*

### **<a name="firmware-rollout-3"></a>Distribuição de firmware (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FailedMaintenanceMode
* Severity: aviso
* Motivo: *"No momento não é possível fazer progresso durante a execução de firmware distribuição."*
* RecommendedAction: *"Verifique se todos os espaços de armazenamento são saudáveis e que nenhum domínio de falha esteja em modo de manutenção."*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.FirmwareVerifyVersionFaile
* Severity: aviso
* Motivo: *"distribuição Firmware foi cancelada devido a informações de versão de firmware ilegível ou inesperado depois de aplicar uma atualização de firmware".*
* RecommendedAction: *"firmware de reinicialização distribuímos o depois que o problema de firmware foi resolvido."*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType: Microsoft.Health.FaultType.FaultDomain.TooManyFailedUpdates
* Severity: aviso
* Motivo: *"distribuição Firmware foi cancelada devido a muitos discos físicos falha uma tentativa de atualização de firmware".*
* RecommendedAction: *"firmware de reinicialização distribuímos o depois que o problema de firmware foi resolvido."*

### <a name="storage-qos-3sup2sup"></a>**Armazenamento QoS (3)**<sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType: Microsoft.Health.FaultType.StorQos.InsufficientThroughput
* Severity: aviso
* Motivo: *"taxa de transferência de armazenamento é insuficiente para satisfazer reserva-se".*
* RecommendedAction: *"Reconfigurar políticas de armazenamento QoS".*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType: Microsoft.Health.FaultType.StorQos.LostCommunication
* Severity: aviso
* Motivo: *"o Gerenciador de política de armazenamento QoS perdeu a comunicação com o volume."*
* RecommendedAction: *"Reinicialize nós {nós}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType: Microsoft.Health.FaultType.StorQos.MisconfiguredFlow
* Severity: aviso
* Motivo: *"um ou mais consumidores de armazenamento (geralmente máquinas virtuais) estão usando uma política de não existente com id {id}".*
* RecommendedAction: *"Recriar quaisquer políticas de armazenamento QoS ausentes".*

<sup>1</sup> indica o volume atingiu 80% completo (gravidade secundária) ou 90% completo (gravidade principal).  
<sup>2</sup> indica alguns .vhd(s) no volume não atendidos seus IOPS mínimo de 10% (menor), 30% (principal) ou 50% (crítico) da janela de 24 horas rolante acima.  

>[!NOTE]
> A integridade dos componentes de compartimento de armazenamento, como os fãs, alimentação e sensores é derivada de SCSI compartimento serviços SES (). Se o fornecedor não fornecer essas informações, o serviço de integridade não é possível exibi-lo.  

## <a name="see-also"></a>Consulte também

- [Serviço de integridade no Windows Server 2016](health-service-overview.md)
