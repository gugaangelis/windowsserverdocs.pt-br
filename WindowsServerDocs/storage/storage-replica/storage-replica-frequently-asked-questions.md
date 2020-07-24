---
title: Perguntas frequentes sobre Réplica de Armazenamento
ms.prod: windows-server
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/15/2020
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: d9e0a3855c324ca9b5c7de90d78535024a0a7a9d
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86964048"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Perguntas frequentes sobre Réplica de Armazenamento

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

Este tópico contém respostas às perguntas frequentes sobre Réplica de Armazenamento.

## <a name="is-storage-replica-supported-on-azure"></a><a name="FAQ1"></a>Há suporte para réplica de armazenamento no Azure?
Sim. Você pode usar os seguintes cenários com o Azure:

1. Replicação de servidor para servidor dentro do Azure (de forma síncrona ou assíncrona entre VMs de IaaS em um ou dois domínios de falha do datacenter, ou de forma assíncrona entre duas regiões separadas)
2. Replicação assíncrona de servidor para servidor entre o Azure e o local (usando VPN ou Azure ExpressRoute)
3. Replicação de cluster para cluster dentro do Azure (de forma síncrona ou assíncrona entre VMs de IaaS em um ou dois domínios de falha do datacenter, ou de forma assíncrona entre duas regiões separadas)
4. Replicação assíncrona de cluster para cluster entre o Azure e o local (usando VPN ou Azure ExpressRoute)

Mais observações sobre o clustering de convidados no Azure podem ser encontradas em: [implantando clusters convidados da VM IaaS no Microsoft Azure](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

Observações importantes:

1. O Azure não dá suporte a clustering de convidado de VHDX compartilhado, portanto, as máquinas virtuais de cluster de failover do Windows devem usar destinos iSCSI para clustering de reserva de disco persistente do armazenamento compartilhado clássico ou Espaços de Armazenamento Diretos.
2. Há Azure Resource Manager modelos para clustering de réplica de armazenamento baseado em Espaços de Armazenamento Diretos em [criar um espaços de armazenamento diretos clusters SOFS com a réplica de armazenamento para recuperação de desastre em regiões do Azure](https://aka.ms/azure-storage-replica-cluster).  
3. Cluster para comunicação RPC de cluster no Azure (exigido pelas APIs de cluster para conceder acesso entre clusters) requer a configuração de acesso à rede para o CNO. Você deve permitir a porta TCP 135 e o intervalo dinâmico acima da porta TCP 49152. Referência [criando o cluster de failover do Windows Server na VM IaaS do Azure – parte 2, rede e criação](/archive/blogs/askcore/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation).  
4. É possível usar clusters de convidado de dois nós, em que cada nó está usando o iSCSI de loopback para um cluster assimétrico replicado pela réplica de armazenamento. Mas isso provavelmente terá um desempenho muito ruim e deve ser usado apenas para cargas de trabalho ou testes muito limitados.  

## <a name="how-do-i-see-the-progress-of-replication-during-initial-sync"></a><a name="FAQ2"></a>Como fazer ver o progresso da replicação durante a sincronização inicial?  
As mensagens de Evento 1237 mostradas no log de eventos do Administrador de Réplica de Armazenamento no servidor de destino mostram o número de bytes copiados e os bytes restantes a cada 10 segundos. Você também pode usar o contador de desempenho de Réplica de Armazenamento no destino exibindo **\Estatísticas de Réplica de Armazenamento\Total de bytes recebidos** para um ou mais volumes replicados. Também pode consultar o grupo de replicação usando o Windows PowerShell. Por exemplo, esta amostra de comando obtém o nome dos grupos no destino, então consulta um grupo chamado **Replicação 2** a cada 10 segundos para mostrar o progresso:  

```  
Get-SRGroup

do{
    $r=(Get-SRGroup -Name "Replication 2").replicas
    [System.Console]::Write("Number of remaining bytes {0}`n", $r.NumOfBytesRemaining)
    Start-Sleep 10
}until($r.ReplicationStatus -eq 'ContinuouslyReplicating')
Write-Output "Replica Status: "$r.replicationstatus

```  

## <a name="can-i-specify-specific-network-interfaces-to-be-used-for-replication"></a><a name="FAQ3"></a>Posso determinar adaptadores de rede específicas a serem usadas para replicação?  

Sim, usando `Set-SRNetworkConstraint`. Este cmdlet opera na camada da interface e é usado em cenários de cluster e não cluster.  
Por exemplo, com um servidor autônomo (em cada nó):  

```  
Get-SRPartnership  

Get-NetIPConfiguration  
```  
Observe as informações de gateway e interface (em ambos os servidores) e as instruções de parceria. Em seguida, execute:  

```  
Set-SRNetworkConstraint -SourceComputerName sr-srv06 -SourceRGName rg02 -  
SourceNWInterface 2 -DestinationComputerName sr-srv05 -DestinationNWInterface 3 -DestinationRGName rg01  

Get-SRNetworkConstraint  

Update-SmbMultichannelConnection  

```  

Para configurar restrições de rede em um cluster estendido:  

    Set-SRNetworkConstraint -SourceComputerName sr-cluster01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-cluster02 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"

## <a name="can-i-configure-one-to-many-replication-or-transitive-a-to-b-to-c-replication"></a><a name="FAQ4"></a>Posso configurar replicação de um para muitos ou replicação transitiva (a para B para C)?  
Não, a réplica de armazenamento dá suporte apenas a uma replicação de um servidor, cluster ou nó de cluster de ampliação. Isso pode mudar em uma versão posterior. Você pode, claro, configurar a replicação entre vários servidores de um par de volumes específico, em qualquer direção. Por exemplo, o Servidor 1 pode replicar seu volume D no Servidor 2, e seu volume E do Servidor 3.

## <a name="can-i-grow-or-shrink-replicated-volumes-replicated-by-storage-replica"></a><a name="FAQ5"></a> Posso aumentar ou reduzir os volumes replicados pela Réplica de Armazenamento?  
Você pode aumentar (expandir) os volumes, mas não pode reduzi-los. Por padrão, a Réplica de Armazenamento impede que os administradores estendam os volumes replicados; use a opção `Set-SRGroup -AllowVolumeResize $TRUE` no grupo de origem, antes do redimensionamento. Por exemplo:

1. Use no computador de origem:`Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Aumentar o volume usando qualquer técnica de sua preferência
3. Use no computador de origem:`Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="can-i-bring-a-destination-volume-online-for-read-only-access"></a><a name="FAQ6"></a>Posso colocar online um volume de destino para acesso somente leitura?  
Não no Windows Server 2016. Armazenamento réplica desmonta o volume de destino quando a replicação começa. 

No entanto, no canal semianual do Windows Server 2019 e do Windows Server, a partir da versão 1709, a opção de montar o armazenamento de destino agora é possível – esse recurso é chamado de "failover de teste". Para fazer isso, você deve ter um volume não utilizado, com formatação NTFS ou ReFS, que não está atualmente replicando no destino. Em seguida, você pode montar um instantâneo do armazenamento replicado em nós de destino temporariamente para fins de teste ou backup. 

Por exemplo, para criar um failover de teste onde você replica um volume "D:" no Grupo de replicação "RG2" do servidor de destino "SRV2" e tem uma unidade "T:" em SRV2 que não está sendo duplicada:

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
O volume replicado D: agora é acessível em SRV2. Você pode ler e gravar normalmente, copiar arquivos para fora dele ou executar um backup online que você salvar em outro lugar para fins de proteção, sob o caminho D:. O volume T: só conterá dados de log.

Para remover o instantâneo de failover de teste e descartar suas alterações:

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
Você deve usar somente o recurso de failover de teste para operações temporárias em curto prazo. Ele não se destina ao uso de longo prazo. Quando estiver em uso, a replicação continua no volume de destino real. 

## <a name="can-i-configure-scale-out-file-server-sofs-in-a-stretch-cluster"></a><a name="FAQ7"></a>Posso configurar o SOFS (servidor de arquivos de escalabilidade horizontal) em um cluster de ampliação?  
Embora tecnicamente possível, essa não é uma configuração recomendada devido à falta de reconhecimento de site nos nós de computação que entram em contato com o SOFS. Se estiver usando rede de distância de campus, em que as latências normalmente são de submilissegundos, essa configuração normalmente funciona sem problemas.   

Se for configurada a replicação de cluster para cluster, a Réplica de Armazenamento dará suporte completo a Servidores de Arquivos de Escalabilidade Horizontal, incluindo o uso de Espaços de Armazenamento Direto, ao replicar entre dois clusters.  

## <a name="is-csv-required-to-replicate-in-a-stretch-cluster-or-between-clusters"></a><a name="FAQ7.5"></a>O CSV é necessário para replicar em um cluster de ampliação ou entre clusters?  
Não. Você pode replicar com o CSV ou a reserva de disco persistente (Laos) de propriedade de um recurso de cluster, como uma função de servidor de arquivos. 

Se for configurada a replicação de cluster para cluster, a Réplica de Armazenamento dará suporte completo a Servidores de Arquivos de Escalabilidade Horizontal, incluindo o uso de Espaços de Armazenamento Direto, ao replicar entre dois clusters.  

## <a name="can-i-configure-storage-spaces-direct-in-a-stretch-cluster-with-storage-replica"></a><a name="FAQ8"></a>Posso configurar Espaços de Armazenamento Direto em um cluster estendido com a Réplica de Armazenamento?  
Essa não é uma configuração com suporte no Windows Server. Isso pode mudar em uma versão posterior. Se for configurada a replicação de cluster para cluster, a Réplica de Armazenamento dará suporte completo a Servidores de Arquivos de Escalabilidade Horizontal e Servidores Hyper-V, incluindo o uso de Espaços de Armazenamento Direto.  

## <a name="how-do-i-configure-asynchronous-replication"></a><a name="FAQ9"></a>Como configurar a replicação assíncrona?  

Especifique `New-SRPartnership -ReplicationMode` e forneça o argumento **Asynchronous**. Por padrão, toda replicação na Réplica de Armazenamento é síncrona. Você também pode alterar o modo com `Set-SRPartnership -ReplicationMode`.  

## <a name="how-do-i-prevent-automatic-failover-of-a-stretch-cluster"></a><a name="FAQ10"></a>Como impedir o failover automático de um cluster estendido?  
Para evitar o failover automático, você pode usar o PowerShell para configurar `Get-ClusterNode -Name "NodeName").NodeWeight=0`. Isso remove o voto em cada nó no local de recuperação de desastre. Você pode usar `Start-ClusterNode -PreventQuorum` em nós no local principal e `Start-ClusterNode -ForceQuorum` em nós no local de desastre para forçar o failover. Não há uma opção gráfica para evitar o failover automático, não é recomendado e impedir o failover automático.  

## <a name="how-do-i-disable-virtual-machine-resiliency"></a><a name="FAQ11"></a>Como desabilitar a resiliência de máquina virtual?
Para impedir que o novo recurso de resiliência de máquina virtual do Hyper-V seja executado e, portanto, pausar máquinas virtuais em vez de fazer o failover para o site de recuperação de desastre, execute`(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="how-can-i-reduce-time-for-initial-synchronization"></a><a name="FAQ12"></a>Como reduzir o tempo para a sincronização inicial?

Você pode usar o armazenamento provisionado como uma maneira de agilizar os tempos de sincronização inicial. A Réplica de Armazenamento consulta e usa automaticamente o armazenamento provisionado dinâmico, incluindo Espaços de Armazenamento sem clusters, discos dinâmicos Hyper-V e LUNs SAN.  

Você também pode usar volumes de dados propagados para reduzir o uso da largura de banda e, às vezes, o tempo, garantindo que o volume de destino tenha algum subconjunto de dados do primário, usando a opção propagada no Gerenciador de Cluster de Failover ou `New-SRPartnership` . Se o volume estiver quase vazio, usar a sincronização propagada pode reduzir o uso de largura de banda e economizar tempo. Há várias maneiras de propagar dados, com graus variados de eficácia:

1. Replicação anterior – replicando com sincronização inicial normal localmente entre os nós que contêm os discos e volumes, removendo a replicação, enviando os discos de destino em outro lugar e, em seguida, adicionando a replicação com a opção propagada. Esse é o método mais eficaz como a réplica de armazenamento garantindo um espelho de cópia de bloco e a única coisa a ser replicada são blocos Delta.
2. Instantâneo restaurado ou backup baseado em instantâneo restaurado – restaurando um instantâneo baseado em volume para o volume de destino, deve haver diferenças mínimas no layout do bloco. Esse é o próximo método mais eficaz, pois os blocos são provavelmente compatíveis graças aos instantâneos de volume sendo imagens espelhadas.
3. Arquivos copiados – criando um novo volume no destino que nunca foi usado antes e executando uma cópia de árvore do Robocopy/MIR completa dos dados, provavelmente haverá correspondências de bloco. Usar o explorador de arquivos do Windows ou copiar uma parte da árvore não criará muitas correspondências de bloco. Copiar arquivos manualmente é o método menos efetivo de propagação.

## <a name="can-i-delegate-users-to-administer-replication"></a><a name="FAQ13"></a>Posso delegar os usuários para administrar a replicação?  

Você pode usar o `Grant-SRDelegation` cmdlet. Isso permite que você defina usuários específicos em cenários de replicação de servidor para servidor, cluster para cluster e cluster estendido com as permissões para criar, alterar ou remover a replicação, sem serem membros do grupo de administradores locais. Por exemplo:  

    Grant-SRDelegation -UserName contso\tonywang  

O cmdlet o lembrará de que o usuário precisa fazer logoff e logon do servidor que pretende administrar para que a alteração tenha efeito. Você pode usar `Get-SRDelegation` e `Revoke-SRDelegation` para controlar isso.  

## <a name="what-are-my-backup-and-restore-options-for-replicated-volumes"></a><a name="FAQ13"></a>Quais são minhas opções de backup e restauração para volumes replicados?
A Réplica de Armazenamento oferece suporte a backup e restauração do volume de origem. Também à criação e restauração de instantâneos do volume de origem. Você não pode fazer backup nem restaurar o volume de destino enquanto ele é protegido pela Réplica de Armazenamento, pois ele não está montado nem acessível. Se ocorrer um desastre em que o volume de origem seja perdido, o uso de `Set-SRPartnership` para promover o volume de destino anterior para ser, agora, uma origem de leitura/gravação permitirá que você faça backup ou restaure o volume. Também é possível remover a replicação com `Remove-SRPartnership` e `Remove-SRGroup` para remontar o volume como leitura/gravação.
Para criar instantâneos de aplicativos consistentes e periódicos, você pode usar VSSADMIN.EXE no servidor de origem para criar instantâneos de volumes de dados replicados. Por exemplo, você está replicando o volume F: com a Réplica de Armazenamento:

    vssadmin create shadow /for=F:
Em seguida, depois de alternar a direção da replicação, remover a replicação ou se simplesmente ainda estiver no mesmo volume de origem, é possível restaurar qualquer instantâneo para seu ponto no tempo. Por exemplo, ainda usando F:

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
Você também pode agendar essa ferramenta para execução periódica usando uma tarefa agendada. Para saber mais sobre como usar o VSS, consulte [Vssadmin](../../administration/windows-commands/vssadmin.md). O backup de volumes de log não é necessário nem tem valor. Tentativas de fazer isso serão ignoradas pelo VSS.
O uso do Backup do Windows Server, do Backup do Microsoft Azure, do Microsoft DPM ou de outras tecnologias de instantâneos, VSS, máquina virtual ou baseadas em arquivo têm suporte na Réplica de Armazenamento contanto que operem na camada do volume. A Réplica de Armazenamento não dá suporte ao backup e à restauração baseados em bloco.

## <a name="can-i-configure-replication-to-restrict-bandwidth-usage"></a><a name="FAQ14"></a>Posso configurar a replicação para restringir o uso de largura de banda?
Sim, por meio do limitador de largura de banda do SMB. Essa é uma configuração global para todo o tráfego de Réplica de Armazenamento e, assim, afeta toda a replicação desse servidor. Normalmente, isso é necessário apenas com a configuração de sincronização inicial da Réplica de Armazenamento, em que todos os dados do volume devem ser transferidos. Se for necessário após a sincronização inicial, a largura de banda de rede será muito baixa para sua carga de trabalho de E/S. Reduza a E/S ou aumente a largura de banda.

Isso só deve ser usado com a replicação assíncrona (observação: a sincronização inicial é sempre assíncrona, mesmo que você tenha especificado que ela seja síncrona).
Você também pode usar políticas de QoS de rede para moldar o tráfego de Réplica de Armazenamento. O uso da replicação de Réplica de Armazenamento com propagação altamente correspondente também reduzirá consideravelmente o uso de largura de banda de sincronização inicial geral.


Para definir o limite de largura de banda, use:

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

Para ver o limite de largura de banda, use:

    Get-SmbBandwidthLimit -Category StorageReplication

Para remover o limite de largura de banda, use:

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="what-network-ports-does-storage-replica-require"></a><a name="FAQ15"></a>Quais portas de rede são exigidas pela Réplica de Armazenamento?
A Réplica de Armazenamento depende de SMB e WSMAN para replicação e gerenciamento. Isso significa que as seguintes portas são necessárias:

 445 (SMB-protocolo de transporte de replicação, protocolo de gerenciamento de RPC de cluster) 5445 (iWARP SMB – somente necessário ao usar a rede RDMA iWARP) 5985 (protocolo de gerenciamento de WSManHTTP para WMI/CIM/PowerShell)

Observação: O cmdlet Test-SRTopology requer ICMPv4/ICMPv6, mas não para replicação nem gerenciamento.

## <a name="what-are-the-log-volume-best-practices"></a><a name="FAQ15.5"></a>Quais são as práticas recomendadas de volume do log?
O tamanho ideal do log varia muito por ambiente e carga de trabalho e é determinado pela quantidade de e/s de gravação que sua carga de trabalho realiza. 

1.  Um log maior ou menor não torna você mais rápido ou mais lento
2.  Um log maior ou menor não tem nenhuma relação em um volume de dados de 10 GB em comparação com um volume de dados de 10 TB, por exemplo

Um log maior simplesmente coleta e retém mais gravações IOs antes de serem encapsulados. Isso permite que uma interrupção no serviço entre o computador de origem e destino – como uma interrupção na rede ou de destino sendo offline - dure mais tempo. Se o log palpáveis 10 horas de gravações, e a rede cai para 2 horas, quando a rede retorna que a origem pode simplesmente reproduzir o delta das alterações de volta para o destino muito rápido e você está protegido novamente muito rapidamente. Se o log contém 10 horas e a interrupção é 2 dias, a fonte agora tem a reprodução de um log diferente chamado o bitmap – e provavelmente será mais lento para entrar novamente em sincronização. Depois que estiver em sincronia, ele volta a usar o log.

A Réplica de Armazenamento depende do log para todo o desempenho da gravação. Desempenho de log essencial para o desempenho de replicação. Certifique-se de que o volume de log tem melhor desempenho que o volume de dados, como o log será serializar e sequentialize todos e/s de gravação. Você sempre deve usar uma mídia flash, como SSD, em volumes de log. Você nunca deve permitir que outras cargas de trabalho sejam executadas no volume do log. Da mesma maneira, você nunca deve permitir que outras cargas de trabalho sejam executadas em volumes de log do banco de dados SQL. 

Novamente: Microsoft recomenda veementemente que o armazenamento de log seja mais rápido do que o armazenamento de dados e volumes de log nunca devem ser usados para outras cargas de trabalho.

Você pode obter recomendações de tamanho de log executando a ferramenta Test-SRTopology. Como alternativa, você pode usar contadores de desempenho em servidores existentes para tornar um tamanho de log Judgement. A fórmula é simples: monitore a taxa de transferência do disco de dados (média de bytes de gravação/s) na carga de trabalho e use-a para calcular a quantidade de tempo que levará para preencher o log de tamanhos diferentes. Por exemplo, a taxa de transferência do disco de dados de 50 MB/s fará com que o log de discos de 120 seja quebrado em 120 GB/50 MB segundos ou 2400 segundos ou 40 minutos. Portanto, a quantidade de tempo que o servidor de destino pode ficar inacessível antes de o log encapsulado é de 40 minutos. Se o log estiver encapsulado, mas o destino se tornar acessível novamente, a origem reproduziria blocos por meio do log de mapa de bits em vez do log principal. O tamanho do log não tem efeito sobre o desempenho.

É necessário fazer backup somente do disco de dados do cluster de origem. Não é necessário fazer backup dos discos de log da réplica de armazenamento, pois um backup pode entrar em conflito com as operações de réplica de armazenamento.

## <a name="why-would-you-choose-a-stretch-cluster-versus-cluster-to-cluster-versus-server-to-server-topology"></a><a name="FAQ16"></a> Por que você escolha um cluster estendido versus cluster ao cluster versus topologia de servidor a servidor?  
A réplica de armazenamento vem em três configurações principais: cluster de ampliação, cluster para cluster e servidor para servidor. Há várias vantagens a cada um.

A topologia de cluster estendido é ideal para exigir que o failover automático com coordenação, como clusters de nuvem privada do Hyper-V e SQL Server FCI as cargas de trabalho. Ele também tem uma interface gráfica interna usando o Gerenciador de Cluster de Failover. Ele utiliza o clássico assimétrico arquitetura de armazenamento compartilhado de espaços de armazenamento, SAN, iSCSI, do cluster e RAID via reserva persistente. Você pode executar isso com um mínimo de 2 nós.

A topologia de cluster ao cluster usa dois clusters separados e é ideal para os administradores que desejam failover manual, especialmente quando o segundo local é provisionado para uso não diário e recuperação de desastres. Coordenação é manual. Ao contrário do cluster de ampliação, Espaços de Armazenamento Diretos pode ser usada nessa configuração (com advertências-consulte as perguntas frequentes sobre réplica de armazenamento e a documentação de cluster para cluster). Você pode executar isso com um mínimo de quatro nós. 

A topologia de servidor a servidor é ideal para clientes que executam o hardware que não pode ser agrupado. Isso requer coordenação e failover manual. É ideal para implantações de baixo custo entre filiais e data centers centrais, especialmente ao usar a replicação assíncrona. Essa configuração geralmente pode substituir a instâncias de servidores de arquivos protegidos DFSR usado para cenários de recuperação de desastres de mestre único.

Em todos os casos, as topologias suportam a ambas as em execução no hardware físico, bem como em máquinas virtuais. Quando estiver em máquinas virtuais, o hipervisor subjacente não exige o Hyper-V; ele pode ser VMware, KVM, Xen, etc.

Armazenamento réplica também tem um modo de servidor-para-self, onde você apontar replicação em dois volumes diferentes no mesmo computador.

## <a name="is-data-deduplication-supported-with-storage-replica"></a><a name="FAQ18"></a>A eliminação de duplicação de dados é suportada com a réplica de armazenamento?

Sim, o Deduplcation de dados tem suporte com a réplica de armazenamento. Habilitar a eliminação de duplicação de dados em um volume no servidor de origem e, durante a replicação, o servidor de destino recebe uma cópia com eliminação de duplicação do volume.

Embora você deva *instalar* a eliminação de duplicação de dados nos servidores de origem e de destino (consulte [Instalando e habilitando a eliminação de duplicação de dados](../data-deduplication/install-enable.md)), é importante não *habilitar* a eliminação de duplicação de dados no servidor de destino. A réplica de armazenamento permite gravações somente no servidor de origem. Como a eliminação de duplicação de dados faz gravações no volume, ele deve ser executado somente no servidor de origem. 

## <a name="can-i-replicate-between-windows-server-2019-and-windows-server-2016"></a><a name="FAQ19"></a>Posso replicar entre o Windows Server 2019 e o Windows Server 2016?

Infelizmente, não há suporte para a criação de uma *nova* parceria entre o windows Server 2019 e o windows Server 2016. Você pode atualizar com segurança um servidor ou cluster que executa o Windows Server 2016 para o Windows Server 2019 e qualquer parceria *existente* continuará funcionando.

No entanto, para obter o desempenho de replicação aprimorado do Windows Server 2019, todos os membros da parceria devem executar o Windows Server 2019 e você deve excluir as parcerias existentes e os grupos de replicação associados e recriá-los com os dados propagados (ao criar a parceria no centro de administração do Windows ou com o cmdlet New-SRPartnership).

## <a name="how-do-i-report-an-issue-with-storage-replica-or-this-guide"></a><a name="FAQ17"></a>Como fazer relatar um problema com a réplica de armazenamento ou este guia?  
Para obter assistência técnica para a Réplica de Armazenamento, poste nos [fóruns do Microsoft TechNet](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview). Você também pode enviar um email a srfeed@microsoft.com perguntas sobre a Réplica de Armazenamento ou problemas com esta documentação. O <https://windowsserver.uservoice.com> site é preferido para solicitações de alteração de design, pois permite que seus colegas de atendimento forneçam suporte e comentários para suas ideias.

## <a name="can-storage-replica-be-configured-to-replicate-in-both-directions"></a><a name="FAQ18"></a>A réplica de armazenamento pode ser configurada para replicar em ambas as direções?
A réplica de armazenamento é uma tecnologia de replicação unidirecional.  Ela só será replicada da origem para o destino por volume.  Essa direção pode ser revertida a qualquer momento, mas ainda está em apenas uma direção.  No entanto, isso não significa que você não pode ter um conjunto de volumes (origem e destino) replicar em uma direção e um conjunto diferente de unidades (origem e destino) replicar na direção oposta.  Por exemplo, você deseja ter a replicação de servidor para servidor configurada.  Server1 e Server2 têm letras de unidade L:, M:, N: e O: e você deseja replicar a unidade M: de Server1 para server2, mas Drive O: replicar de Server2 para Server1.  Isso pode ser feito desde que haja unidades de log separadas para cada um dos grupos. ,. 

- Unidade de origem do Server1 M: com a unidade de log de origem L: replicando para a unidade de destino do Server2 M: com a unidade de log de destino L:
- Unidade de origem do Server2 O: com a unidade de log de origem N: replicando para Server1 unidade de destino O: com a unidade de log de destino N:

## <a name="related-topics"></a>Tópicos Relacionados  
- [Visão geral da Réplica de Armazenamento](storage-replica-overview.md) 
- [Estender a replicação do cluster usando o armazenamento compartilhado](stretch-cluster-replication-using-shared-storage.md)  
- [Replicação de armazenamento de servidor para servidor](server-to-server-storage-replication.md)  
- [Cluster para replicação de armazenamento de cluster](cluster-to-cluster-storage-replication.md)  
- [Réplica de armazenamento: problemas conhecidos](storage-replica-known-issues.md)  

## <a name="see-also"></a>Consulte Também  
- [Visão geral do Armazenamento](../storage.yml)  
- [Espaços de Armazenamento Diretos](../storage-spaces/storage-spaces-direct-overview.md)  
