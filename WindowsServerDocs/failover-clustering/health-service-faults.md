---
title: Falhas de Serviço de Integridade
ms.prod: windows-server
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
author: cosmosdarwin
ms.date: 10/05/2017
ms.openlocfilehash: 913a596a46720718a165295345cb02e3e2baa1de
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80827559"
---
# <a name="health-service-faults"></a>Falhas de Serviço de Integridade
> Aplica-se a: Windows Server 2019, Windows Server 2016

## <a name="what-are-faults"></a>O que são falhas

O Serviço de Integridade monitora constantemente o cluster Espaços de Armazenamento Diretos para detectar problemas e gerar "falhas". Um novo cmdlet exibe quaisquer falhas atuais, permitindo que você verifique facilmente a integridade da sua implantação sem examinar cada entidade ou recurso por vez. As falhas são projetadas para serem precisas, fáceis de entender e acionáveis.  

Cada falha contém cinco campos importantes:  

-   Severity
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

O Serviço de Integridade pode avaliar o potencial causalidade entre entidades com falha para identificar e combinar falhas que são consequências do mesmo problema subjacente. Reconhecendo cadeias de efeito, isso gera relatórios menos longos. Por exemplo, se um servidor estiver inativo, espera-se que qualquer unidade no servidor também seja sem conectividade. Portanto, apenas uma falha será gerada para a causa raiz-nesse caso, o servidor.  

## <a name="usage-in-powershell"></a>Uso no PowerShell

Para ver as falhas atuais no PowerShell, execute este cmdlet:

```PowerShell
Get-StorageSubSystem Cluster* | Debug-StorageSubSystem  
```

Isso retorna quaisquer falhas que afetam o cluster de Espaços de Armazenamento Diretos geral. Geralmente, essas falhas estão relacionadas ao hardware ou à configuração. Se não houver falhas, esse cmdlet não retornará nada.  

>[!NOTE]
> Em um ambiente de não produção e, por sua conta e risco, você pode experimentar esse recurso disparando falhas por conta própria, por exemplo, removendo um disco físico ou desligando um nó. Depois que a falha for exibida, insira novamente o disco físico ou reinicie o nó e a falha desaparecerá novamente.

Você também pode exibir as falhas que estão afetando apenas volumes específicos ou compartilhamentos de arquivos com os seguintes cmdlets:  

```PowerShell
Get-Volume -FileSystemLabel <Label> | Debug-Volume  

Get-FileShare -Name <Name> | Debug-FileShare  
```

Isso retorna todas as falhas que afetam apenas o volume ou o compartilhamento de arquivos específico. Geralmente, essas falhas estão relacionadas ao planejamento de capacidade, à resiliência de dados ou a recursos como armazenamento de qualidade de serviço ou de réplica de armazenamento. 

## <a name="usage-in-net-and-c"></a>Uso no .NET eC#

### <a name="connect"></a>Conectar

Para consultar o Serviço de Integridade, será necessário estabelecer um **CimSession** com o cluster. Para fazer isso, você precisará de algumas coisas que estão disponíveis apenas no .NET completo, o que significa que não é possível fazer isso prontamente diretamente de um aplicativo Web ou móvel. Esses exemplos de código usarão o C\#, a opção mais direta para essa camada de acesso a dados.

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

É recomendável que você construa a **SecureString** de senha diretamente da entrada do usuário em tempo real, para que sua senha nunca seja armazenada na memória em texto não criptografado. Isso ajuda a atenuar uma variedade de questões de segurança. Mas, na prática, construí-lo como acima é comum para fins de criação de protótipos.

### <a name="discover-objects"></a>Descobrir objetos

Com o **CimSession** estabelecido, você pode consultar Instrumentação de gerenciamento do Windows (WMI) no cluster.

Antes que você possa obter falhas ou métricas, você precisará obter instâncias de vários objetos relevantes. Primeiro, o **MSFT\_StorageSubSystem** que representa espaços de armazenamento diretos no cluster. Usando isso, você pode obter cada **msft\_StorageNode** no cluster e todos os\_de dados do **MSFT** Por fim, você precisará do **MSFT\_StorageHealth**, o serviço de integridade em si.

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

Esses são os mesmos objetos que você obtém no PowerShell usando cmdlets como **Get-StorageSubSystem**, **Get-StorageNode**e **Get-volume**.

Você pode acessar todas as mesmas propriedades, documentadas em [classes de API de gerenciamento de armazenamento](https://msdn.microsoft.com/library/windows/desktop/hh830612(v=vs.85).aspx).

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

Invoque o **diagnóstico** para obter quaisquer falhas atuais com escopo para o **CimInstance**de destino, que é o cluster ou qualquer volume.

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

### <a name="optional-myfault-class"></a>Opcional: classe myfault

Pode fazer sentido criar e manter sua própria representação de falhas. Por exemplo, essa classe **myfault** armazena várias propriedades-chave de falhas, incluindo **faultid**, que pode ser usado posteriormente para associar atualizações ou remover notificações, ou para eliminar a duplicação caso a mesma falha seja detectada várias vezes, por qualquer motivo.

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

A lista completa de propriedades em cada falha (**DiagnoseResult**) está documentada abaixo.

### <a name="fault-events"></a>Eventos de falha

Quando as falhas são criadas, removidas ou atualizadas, o Serviço de Integridade gera eventos WMI. Eles são essenciais para manter o estado do aplicativo em sincronia sem sondagem frequente e podem ajudar com coisas como determinar quando enviar alertas por email, por exemplo. Para assinar esses eventos, este código de exemplo usa o padrão de design observador novamente.

Primeiro, assine o **MSFT\_eventos StorageFaultEvent** .

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

Em seguida, implemente um observador cujo método **OnNext ()** será invocado sempre que um novo evento for gerado.

Cada evento contém **ChangeType** que indica se uma falha está sendo criada, removida ou atualizada e a **faultid**relevante.

Além disso, eles contêm todas as propriedades da própria falha.

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

### <a name="understand-fault-lifecycle"></a>Entender o ciclo de vida da falha

As falhas não devem ser marcadas como "vistas" ou resolvidas pelo usuário. Eles são criados quando o Serviço de Integridade observa um problema e eles são removidos automaticamente e somente quando o Serviço de Integridade não pode mais observar o problema. Em geral, isso reflete que o problema foi corrigido.

No entanto, em alguns casos, as falhas podem ser redescobertas pelo Serviço de Integridade (por exemplo, após o failover ou devido à conectividade intermitente, etc.). Por esse motivo, talvez faça sentido persistir sua própria representação de falhas, para que você possa facilmente eliminar a duplicação. Isso é especialmente importante se você enviar alertas por email ou equivalentes.

### <a name="properties-of-faults"></a>Propriedades de falhas

Esta tabela apresenta várias propriedades-chave do objeto de falha. Para o esquema completo, inspecione a classe **MSFT\_StorageDiagnoseResult** em *storagewmi. mof*.

| **Propriedade**              | **Exemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| Faultid                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft. Health. FaultType. volume. Capacity                      |
| Reason                    | "O volume está ficando sem espaço disponível."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, slot 11                                        |
| RecommendedActions        | {"Expandir o volume"., "migrar cargas de trabalho para outros volumes".}   |

**Faultid** Exclusivo no escopo de um cluster.

**PerceivedSeverity** PerceivedSeverity = {4, 5, 6} = {"informativo", "aviso" e "erro"}, ou cores equivalentes, como azul, amarelo e vermelho.

**FaultingObjectDescription** Informações de parte de hardware, normalmente em branco para objetos de software.

**FaultingObjectLocation** Informações de local para hardware, normalmente em branco para objetos de software.

**RecommendedActions** Lista de ações recomendadas, que são independentes e sem ordem específica. Hoje, essa lista costuma ser de comprimento 1.

## <a name="properties-of-fault-events"></a>Propriedades de eventos de falha

Esta tabela apresenta várias propriedades-chave do evento de falha. Para o esquema completo, inspecione a classe **MSFT\_StorageFaultEvent** em *storagewmi. mof*.

Observe o **ChangeType**, que indica se uma falha está sendo criada, removida ou atualizada e a **faultid**. Um evento também contém todas as propriedades da falha afetada.

| **Propriedade**              | **Exemplo**                                                     |
|---------------------------|-----------------------------------------------------------------|
| ChangeType                | 0                                                               |
| Faultid                   | {12345-12345-12345-12345-12345}                                 |
| FaultType                 | Microsoft. Health. FaultType. volume. Capacity                      |
| Reason                    | "O volume está ficando sem espaço disponível."                 |
| PerceivedSeverity         | 5                                                               |
| FaultingObjectDescription | Contoso XYZ9000 S.N. 123456789                                  |
| FaultingObjectLocation    | Rack A06, RU 25, slot 11                                        |
| RecommendedActions        | {"Expandir o volume"., "migrar cargas de trabalho para outros volumes".}   |

**ChangeType** ChangeType = {0, 1, 2} = {"criar", "remover", "atualizar"}.

## <a name="coverage"></a>Cobertura

No Windows Server 2016, o Serviço de Integridade fornece a seguinte cobertura de falha:  

### <a name="physicaldisk-8"></a>**PhysicalDisk (8)**

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedmedia"></a>FaultType: Microsoft. Health. FaultType. PhysicalDisk. FailedMedia
* Gravidade: Aviso
* Motivo: *"o disco físico falhou."*
* Recomendado: *"substituir o disco físico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldisklostcommunication"></a>FaultType: Microsoft. Health. FaultType. PhysicalDisk. LostCommunication
* Gravidade: Aviso
* Motivo: *"a conectividade foi perdida no disco físico".*
* Recomendado: *"Verifique se o disco físico está funcionando e conectado corretamente".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunresponsive"></a>FaultType: Microsoft. Health. FaultType. PhysicalDisk. sem resposta
* Gravidade: Aviso
* Motivo: *"o disco físico está apresentando uma falta de resposta recorrente".*
* Recomendado: *"substituir o disco físico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskpredictivefailure"></a>FaultType: Microsoft. Health. FaultType. PhysicalDisk. PredictiveFailure
* Gravidade: Aviso
* Motivo: *"uma falha do disco físico é prevista para ocorrer em breve".*
* Recomendado: *"substituir o disco físico".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedhardware"></a>FaultType: Microsoft. Health. FaultType. PhysicalDisk. UnsupportedHardware
* Gravidade: Aviso
* Motivo: *"o disco físico está em quarentena porque não há suporte para ele no fornecedor da solução".*
* Recomendado: *"Substitua o disco físico por hardware com suporte".*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunsupportedfirmware"></a>FaultType: Microsoft. Health. FaultType. PhysicalDisk. UnsupportedFirmware
* Gravidade: Aviso
* Motivo: *"o disco físico está em quarentena porque sua versão do firmware não é suportada pelo fornecedor da solução."*
* Recomendado: *"atualizar o firmware no disco físico para a versão de destino."*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskunrecognizedmetadata"></a>FaultType: Microsoft. Health. FaultType. PhysicalDisk. UnrecognizedMetadata
* Gravidade: Aviso
* Motivo: *"o disco físico tem metadados* não reconhecidos."
* Recomendado: *"este disco pode conter dados de um pool de armazenamento desconhecido. Primeiro, verifique se não há dados úteis neste disco e, em seguida, redefina o disco. "*

#### <a name="faulttype-microsofthealthfaulttypephysicaldiskfailedfirmwareupdate"></a>FaultType: Microsoft. Health. FaultType. PhysicalDisk. FailedFirmwareUpdate
* Gravidade: Aviso
* Motivo: *"falha na tentativa de atualizar o firmware no disco físico".*
* Recomendado: *"Tente usar um binário de firmware diferente".*

### <a name="virtual-disk-2"></a>**Disco virtual (2)**

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksneedsrepair"></a>FaultType: Microsoft. Health. FaultType. VirtualDisks. NeedsRepair
* Gravidade: informativo
* Motivo: *"alguns dados neste volume não são totalmente resilientes. Ele permanece acessível. "*
* Recomendado: *"restaurando a resiliência dos dados."*

#### <a name="faulttype-microsofthealthfaulttypevirtualdisksdetached"></a>FaultType: Microsoft. Health. FaultType. VirtualDisks. desanexado
* Severidade: crítico
* Motivo: *"o volume está inacessível. Alguns dados podem ser perdidos. "*
* Recomendado: *"Verifique a conectividade física e/ou de rede de todos os dispositivos de armazenamento. Talvez seja necessário restaurar a partir do backup. "*

### <a name="pool-capacity-1"></a>**Capacidade do pool (1)**

#### <a name="faulttype-microsofthealthfaulttypestoragepoolinsufficientreservecapacityfault"></a>FaultType: Microsoft. Health. FaultType. StoragePool. InsufficientReserveCapacityFault
* Gravidade: Aviso
* Motivo: *"o pool de armazenamento não tem a capacidade de reserva mínima recomendada. Isso pode limitar sua capacidade de restaurar a resiliência de dados no caso de falhas de unidade. "*
* Recomendado: *"adicionar capacidade adicional ao pool de armazenamento ou liberar capacidade. A reserva mínima recomendada varia de acordo com a implantação, mas tem aproximadamente 2 unidades de capacidade. "*

### <a name="volume-capacity-2sup1sup"></a>**Capacidade do volume (2)** <sup>1</sup>

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft. Health. FaultType. volume. Capacity
* Gravidade: Aviso
* Motivo: *"o volume está ficando sem espaço disponível".*
* Recomendado: *"expandir o volume ou migrar cargas de trabalho para outros volumes".*

#### <a name="faulttype-microsofthealthfaulttypevolumecapacity"></a>FaultType: Microsoft. Health. FaultType. volume. Capacity
* Severidade: crítico
* Motivo: *"o volume está ficando sem espaço disponível".*
* Recomendado: *"expandir o volume ou migrar cargas de trabalho para outros volumes".*

### <a name="server-3"></a>**Servidor (3)**

#### <a name="faulttype-microsofthealthfaulttypeserverdown"></a>FaultType: Microsoft. Health. FaultType. Server. Down
* Severidade: crítico
* Motivo: *"o servidor não pode ser acessado".*
* Recomendado: *"iniciar ou substituir servidor".*

#### <a name="faulttype-microsofthealthfaulttypeserverisolated"></a>FaultType: Microsoft. Health. FaultType. Server. Isolated
* Severidade: crítico
* Motivo: *"o servidor está isolado do cluster devido a problemas de conectividade".*
* Recomendado: *"se o isolamento persistir, verifique as redes ou migre cargas de trabalho para outros nós".*

#### <a name="faulttype-microsofthealthfaulttypeserverquarantined"></a>FaultType: Microsoft. Health. FaultType. Server. Quarantineed
* Severidade: crítico
* Motivo: *"o servidor é colocado em quarentena pelo cluster devido a falhas recorrentes".*
* Recomendado: *"substituir o servidor ou corrigir a rede".*

### <a name="cluster-1"></a>**Cluster (1)**

#### <a name="faulttype-microsofthealthfaulttypeclusterquorumwitnesserror"></a>FaultType: Microsoft. Health. FaultType. ClusterQuorumWitness. Error
* Severidade: crítico
* Motivo: *"o cluster é uma falha de um servidor fora de ficar inativo".*
* Recomendado: *"Verifique o recurso de testemunha e reinicie conforme necessário. Iniciar ou substituir servidores com falha. "*

### <a name="network-adapterinterface-4"></a>**Adaptador/interface de rede (4)**

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisconnected"></a>FaultType: Microsoft. Health. FaultType. adaptador. Disconnected
* Gravidade: Aviso
* Motivo: *"a interface de rede foi desconectada".*
* Recomendado: *"reconectar o cabo de rede".*

#### <a name="faulttype-microsofthealthfaulttypenetworkinterfacemissing"></a>FaultType: Microsoft. Health. FaultType. NetworkInterface. Missing
* Gravidade: Aviso
* Motivo: *"o servidor {Server} tem adaptador (s) de rede ausentes conectados à rede de cluster {cluster Network}."*
* Recomendado: *"conectar o servidor à rede de cluster ausente".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterhardware"></a>FaultType: Microsoft. Health. FaultType. adaptador. hardware
* Gravidade: Aviso
* Motivo: *"a interface de rede teve uma falha de hardware."*
* Recomendado: *"substituir o adaptador de interface de rede".*

#### <a name="faulttype-microsofthealthfaulttypenetworkadapterdisabled"></a>FaultType: Microsoft. Health. FaultType. adaptador. Disabled
* Gravidade: Aviso
* Motivo: *"a interface de rede {interface de rede} não está habilitada e não está sendo usada."*
* Recomendado: *"habilitar a interface de rede".*

### <a name="enclosure-6"></a>**Compartimento (6)**

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurelostcommunication"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. LostCommunication
* Gravidade: Aviso
* Motivo: *"a comunicação foi perdida no compartimento de armazenamento."*
* Recomendado: *"iniciar ou substituir o compartimento de armazenamento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurefanerror"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. FanError
* Gravidade: Aviso
* Motivo: *"o ventilador na posição {position} do compartimento de armazenamento falhou".*
* Recomendado: *"Substitua o ventilador no compartimento de armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurecurrentsensorerror"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. CurrentSensorError
* Gravidade: Aviso
* Motivo: *"o sensor atual na posição {position} do compartimento de armazenamento falhou".*
* Recomendado: *"substituir um sensor atual no compartimento de armazenamento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosurevoltagesensorerror"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. VoltageSensorError
* Gravidade: Aviso
* Motivo: *"o sensor de tensão na posição {position} do compartimento de armazenamento falhou".*
* Recomendado: *"substituir um sensor de tensão no compartimento de armazenamento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosureiocontrollererror"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. IoControllerError
* Gravidade: Aviso
* Motivo: *"o controlador de e/s na posição {position} do compartimento de armazenamento falhou".*
* Recomendado: *"substituir um controlador de e/s no compartimento de armazenamento."*

#### <a name="faulttype-microsofthealthfaulttypestorageenclosuretemperaturesensorerror"></a>FaultType: Microsoft. Health. FaultType. StorageEnclosure. TemperatureSensorError
* Gravidade: Aviso
* Motivo: *"o sensor de temperatura na posição {position} do compartimento de armazenamento falhou".*
* Recomendado: *"substituir um sensor de temperatura no compartimento de armazenamento."*

### <a name="firmware-rollout-3"></a>**Distribuição de firmware (3)**

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfailedmaintenancemode"></a>FaultType: Microsoft. Health. FaultType. FaultDomain. FailedMaintenanceMode
* Gravidade: Aviso
* Motivo: *"Atualmente, não é possível fazer o andamento ao executar a distribuição de firmware".*
* Recomendado: *"Verifique se todos os espaços de armazenamento estão íntegros e se nenhum domínio de falha está em modo de manutenção no momento."*

#### <a name="faulttype-microsofthealthfaulttypefaultdomainfirmwareverifyversionfaile"></a>FaultType: Microsoft. Health. FaultType. FaultDomain. FirmwareVerifyVersionFaile
* Gravidade: Aviso
* Motivo: *"a distribuição de firmware foi cancelada devido a informações de versão de firmware ilegível ou inesperadas após a aplicação de uma atualização de firmware".*
* Recomendado: *"reinicie a distribuição do firmware depois que o problema do firmware tiver sido resolvido".*

#### <a name="faulttype-microsofthealthfaulttypefaultdomaintoomanyfailedupdates"></a>FaultType: Microsoft. Health. FaultType. FaultDomain. TooManyFailedUpdates
* Gravidade: Aviso
* Motivo: *"a distribuição do firmware foi cancelada devido a muitos discos físicos falhando em uma tentativa de atualização do firmware".*
* Recomendado: *"reinicie a distribuição do firmware depois que o problema do firmware tiver sido resolvido".*

### <a name="storage-qos-3sup2sup"></a>**QoS de armazenamento (3)** <sup>2</sup>

#### <a name="faulttype-microsofthealthfaulttypestorqosinsufficientthroughput"></a>FaultType: Microsoft. Health. FaultType. StorQos. InsufficientThroughput
* Gravidade: Aviso
* Motivo: *"a taxa de transferência de armazenamento é insuficiente para atender às reservas".*
* Recomendado: *"reconfigurar políticas de QoS de armazenamento".*

#### <a name="faulttype-microsofthealthfaulttypestorqoslostcommunication"></a>FaultType: Microsoft. Health. FaultType. StorQos. LostCommunication
* Gravidade: Aviso
* Motivo: *"o Gerenciador de políticas de QoS de armazenamento perdeu a comunicação com o volume".*
* Recomendado: *"reinicializar nós {Nodes}"*

#### <a name="faulttype-microsofthealthfaulttypestorqosmisconfiguredflow"></a>FaultType: Microsoft. Health. FaultType. StorQos. MisconfiguredFlow
* Gravidade: Aviso
* Motivo: *"um ou mais consumidores de armazenamento (geralmente máquinas virtuais) estão usando uma política inexistente com a ID {ID}."*
* Recomendado: *"recriar quaisquer políticas de QoS de armazenamento ausentes".*

<sup>1</sup> indica que o volume atingiu 80% Full (severidade secundária) ou 90% Full (severidade principal).  
<sup>2</sup> indica que alguns. VHD (s) no volume não atingiram seu IOPS mínimo por mais de 10% (menor), 30% (principal) ou 50% (crítico) de uma janela de 24 horas sem interrupção.  

>[!NOTE]
> A integridade dos componentes do compartimento de armazenamento, como sensores, fontes de alimentação e ventiladores, é derivada de SES (Serviços de Compartimento) SCSI. Se o fornecedor não lhe der essas informações, o Serviço de Integridade não poderá exibi-las.  

## <a name="see-also"></a>Consulte também

- [Serviço de Integridade no Windows Server 2016](health-service-overview.md)
