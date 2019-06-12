---
title: Perguntas frequentes sobre Réplica de Armazenamento
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 04/26/2019
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: d03407292a797b1cd511937ba40fc0fa373f5dc0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447576"
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Perguntas frequentes sobre Réplica de Armazenamento

>Aplica-se a: 2019, Windows Server 2016, Windows Server (canal semestral) do Windows Server

Este tópico contém respostas às perguntas frequentes sobre Réplica de Armazenamento.

## <a name="FAQ1"></a> A réplica de armazenamento com suporte no Azure?
Sim. Você pode usar os cenários a seguir com o Azure:

1. Replicação de servidor para servidor dentro do Azure (forma síncrona ou assíncrona entre as VMs de IaaS em um ou dois domínios de falha do datacenter ou assíncrona entre duas regiões separadas)
2. Replicação assíncrona de servidor para servidor entre o Azure e no local (usando VPN ou ExpressRoute do Azure)
3. Replicação de cluster para cluster dentro do Azure (forma síncrona ou assíncrona entre as VMs de IaaS em um ou dois domínios de falha do datacenter ou assíncrona entre duas regiões separadas)
4. Replicação assíncrona de cluster para cluster entre o Azure e no local (usando VPN ou ExpressRoute do Azure)

Observações adicionais sobre clustering de convidado no Azure podem ser encontradas em: [Implantando Clusters convidados em VMs IaaS no Microsoft Azure](https://techcommunity.microsoft.com/t5/Failover-Clustering/Deploying-IaaS-VM-Guest-Clusters-in-Microsoft-Azure/ba-p/372126).

Observações importantes:

1. Azure não dá suporte a convidado de VHDX compartilhado de cluster, portanto, as máquinas virtuais de Cluster de Failover do Windows deve usar destinos iSCSI para clustering de reserva clássico disco persistente do armazenamento compartilhado ou espaços de armazenamento diretos.
2. Há modelos do Azure Resource Manager para clustering de réplica de armazenamento de espaços de armazenamento diretos com base no [criar um armazenamento de espaços diretos Clusters de SOFS com a réplica de armazenamento para recuperação de desastre entre regiões do Azure](https://aka.ms/azure-storage-replica-cluster).  
3. Cluster para comunicação de RPC de cluster no Azure (exigida pelo cluster APIs para conceder acesso entre o cluster) exige a configuração de acesso à rede para o CNO. Você deve permitir a porta TCP 135 e o intervalo dinâmico acima 49152 a porta TCP. Referência [construção Windows Server Failover Cluster na VM IAAS do Azure – parte 2 de rede e criação](https://blogs.technet.microsoft.com/askcore/2015/06/24/building-windows-server-failover-cluster-on-azure-iaas-vm-part-2-network-and-creation/).  
4. É possível usar clusters de convidados de dois nós, em que cada nó é usando o iSCSI de loopback para um cluster assimétrico replicado pela réplica de armazenamento. Mas isso provavelmente terá um desempenho muito fraco e deve ser usado apenas para cargas de trabalho muito limitadas ou teste.  

## <a name="FAQ2"></a> Como posso ver o andamento da replicação durante a sincronização inicial?  
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

## <a name="FAQ3"></a>Posso especificar interfaces de rede específico a ser usado para replicação?  

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

    Set-SRNetworkConstraint -SourceComputerName sr-srv01 -SourceRGName group1 -SourceNWInterface "Cluster Network 1","Cluster Network 2" -DestinationComputerName sr-srv03 -DestinationRGName group2 -DestinationNWInterface "Cluster Network 1","Cluster Network 2"  

## <a name="FAQ4"></a> Posso configurar a replicação de um-para-muitos ou a replicação transitiva (A para B para C)?  
Não, a réplica de armazenamento dá suporte à replicação de apenas um para cada um de um servidor, cluster ou nó de cluster estendido. Isso pode mudar em uma versão posterior. Você pode, claro, configurar a replicação entre vários servidores de um par de volumes específico, em qualquer direção. Por exemplo, o Servidor 1 pode replicar seu volume D no Servidor 2, e seu volume E do Servidor 3.

## <a name="FAQ5"></a> Pode aumentar ou reduzir os volumes replicados pela réplica de armazenamento?  
Você pode aumentar (expandir) os volumes, mas não pode reduzi-los. Por padrão, a Réplica de Armazenamento impede que os administradores estendam os volumes replicados; use a opção `Set-SRGroup -AllowVolumeResize $TRUE` no grupo de origem, antes do redimensionamento. Por exemplo:

1. Use em relação ao computador de origem: `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Aumentar o volume usando qualquer técnica de sua preferência
3. Use em relação ao computador de origem: `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>Posso colocar online um volume de destino para acesso somente leitura?  
Não no Windows Server 2016. Armazenamento réplica desmonta o volume de destino quando a replicação começa. 

No entanto, no Windows Server 2019 e no Windows Server canal semestral começando com a versão, 1709, a opção de montar o armazenamento de destino agora é possível - esse recurso é chamado de "Failover de teste". Para fazer isso, você deve ter um volume não utilizado, com formatação NTFS ou ReFS, que não está atualmente replicando no destino. Em seguida, você pode montar um instantâneo do armazenamento replicado em nós de destino temporariamente para fins de teste ou backup. 

Por exemplo, para criar um failover de teste onde você replica um volume "D:" no Grupo de replicação "RG2" do servidor de destino "SRV2" e tem uma unidade "T:" em SRV2 que não está sendo duplicada:

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
O volume replicado D: agora é acessível em SRV2. Você pode ler e gravar nele, normalmente, copiar arquivos habilitá-lo ou executar um backup online que você salvar em outro lugar, por questões de segurança, sob o caminho d:. O t: volume conterá apenas dados de log.

Para remover o instantâneo de failover de teste e descartar suas alterações:

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
Você deve usar somente o recurso de failover de teste para operações temporárias em curto prazo. Ele não se destina ao uso de longo prazo. Quando estiver em uso, a replicação continua no volume de destino real. 

## <a name="FAQ7"></a> Pode configurar o servidor de arquivos de escalabilidade horizontal (SOFS) em um cluster estendido?  
Embora seja tecnicamente possível, isso não é uma configuração recomendada devido à falta de reconhecimento de local em nós de computação que contatam o SOFS. Se a distância de campus de rede, onde as latências são, normalmente, inferiores a milissegundos, essa configuração normalmente funciona sem problemas.   

Se for configurada a replicação de cluster para cluster, a Réplica de Armazenamento dará suporte completo a Servidores de Arquivos de Escalabilidade Horizontal, incluindo o uso de Espaços de Armazenamento Direto, ao replicar entre dois clusters.  

## <a name="FAQ7.5"></a> CSV é necessário para replicar em um cluster estendido ou entre clusters?  
Não. Você pode replicar com reserva de disco persistente (PDR) pertencentes a um recurso de cluster, como uma função de servidor de arquivos ou CSV. 

Se for configurada a replicação de cluster para cluster, a Réplica de Armazenamento dará suporte completo a Servidores de Arquivos de Escalabilidade Horizontal, incluindo o uso de Espaços de Armazenamento Direto, ao replicar entre dois clusters.  

## <a name="FAQ8"></a>Posso configurar espaços de armazenamento diretos em um cluster estendido com a réplica de armazenamento?  
Isso não é uma configuração com suporte no Windows Server. Isso pode mudar em uma versão posterior. Se for configurada a replicação de cluster para cluster, a Réplica de Armazenamento dará suporte completo a Servidores de Arquivos de Escalabilidade Horizontal e Servidores Hyper-V, incluindo o uso de Espaços de Armazenamento Direto.  

## <a name="FAQ9"></a>Como configurar a replicação assíncrona?  

Especifique `New-SRPartnership -ReplicationMode` e forneça o argumento **Asynchronous**. Por padrão, toda replicação na Réplica de Armazenamento é síncrona. Você também pode alterar o modo com `Set-SRPartnership -ReplicationMode`.  

## <a name="FAQ10"></a>Como impedir que o failover automático de um cluster estendido?  
Para evitar o failover automático, você pode usar o PowerShell para configurar `Get-ClusterNode -Name "NodeName").NodeWeight=0`. Isso remove o voto em cada nó no local de recuperação de desastre. Você pode usar `Start-ClusterNode -PreventQuorum` em nós no local principal e `Start-ClusterNode -ForceQuorum` em nós no local de desastre para forçar o failover. Não há uma opção gráfica para evitar o failover automático, não é recomendado e impedir o failover automático.  

## <a name="FAQ11"></a>Como desabilitar a resiliência de máquina virtual?
Para impedir que o novo recurso de resiliência de máquina virtual do Hyper-V, da execução e, portanto, pause máquinas virtuais em vez de fazer o failover para o site de recuperação de desastre, execute `(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a> Como reduzir o tempo de sincronização inicial?

Você pode usar o armazenamento provisionado como uma maneira de agilizar os tempos de sincronização inicial. A Réplica de Armazenamento consulta e usa automaticamente o armazenamento provisionado dinâmico, incluindo Espaços de Armazenamento sem clusters, discos dinâmicos Hyper-V e LUNs SAN.  

Você pode também usar volumes de dados propagados para reduzir o uso de largura de banda e, às vezes, de tempo, garantindo que o destino do volume tem algum subconjunto dos dados do principal, em seguida, usando a opção propagado no Gerenciador de Cluster de Failover ou `New-SRPartnership`. Se o volume estiver quase vazio, usar a sincronização propagada pode reduzir o uso de largura de banda e economizar tempo. Há várias maneiras para propagar dados com graus variados de eficácia:

1. Replicação anterior - replicando com inicial normal de sincronização localmente entre os nós que contém os discos e volumes, removendo a replicação, envio dos discos de destino em outro lugar, em seguida, adicionando a replicação com a opção propagada. Isso é o método mais eficaz como um espelho de cópia do bloco a garantia de réplica de armazenamento e a única coisa que a replicação é blocos delta.
2. Restaurado do instantâneo ou restaurado com base no instantâneo de backup – por meio da restauração de um instantâneo de volume para o volume de destino, deve haver diferenças mínimas no layout de bloco. Isso é o próximo método mais eficaz como blocos provavelmente corresponder graças aos instantâneos de volume que está sendo imagens espelhadas.
3. Arquivos copiados - criando um novo volume no destino que nunca foi usado antes e realizando uma árvore /MIR robocopy completo copiar os dados, há probabilidade de serem correspondências de bloco. Usando o Explorador de arquivos do Windows ou copiando uma parte da árvore não criará excessivo de correspondências de bloco. Copiar manualmente os arquivos é o método menos eficiente da propagação.

## <a name="FAQ13"></a> Eu posso delegar usuários para administrar a replicação?  

Você pode usar o `Grant-SRDelegation` cmdlet. Isso permite que você defina usuários específicos em cenários de replicação de servidor para servidor, cluster para cluster e cluster estendido com as permissões para criar, alterar ou remover a replicação, sem serem membros do grupo de administradores locais. Por exemplo:  

    Grant-SRDelegation -UserName contso\tonywang  

O cmdlet o lembrará de que o usuário precisa fazer logoff e logon do servidor que pretende administrar para que a alteração tenha efeito. Você pode usar `Get-SRDelegation` e `Revoke-SRDelegation` para controlar isso.  

## <a name="FAQ13"></a> Quais são minha backup e opções de restauração para volumes replicados?
A Réplica de Armazenamento oferece suporte a backup e restauração do volume de origem. Também à criação e restauração de instantâneos do volume de origem. Você não pode fazer backup nem restaurar o volume de destino enquanto ele é protegido pela Réplica de Armazenamento, pois ele não está montado nem acessível. Se ocorrer um desastre em que o volume de origem seja perdido, o uso de `Set-SRPartnership` para promover o volume de destino anterior para ser, agora, uma origem de leitura/gravação permitirá que você faça backup ou restaure o volume. Também é possível remover a replicação com `Remove-SRPartnership` e `Remove-SRGroup` para remontar o volume como leitura/gravação.
Para criar instantâneos de aplicativos consistentes e periódicos, você pode usar VSSADMIN.EXE no servidor de origem para criar instantâneos de volumes de dados replicados. Por exemplo, você está replicando o volume F: com a Réplica de Armazenamento:

    vssadmin create shadow /for=F:
Em seguida, depois de alternar a direção da replicação, remover a replicação ou se simplesmente ainda estiver no mesmo volume de origem, é possível restaurar qualquer instantâneo para seu ponto no tempo. Por exemplo, ainda usando F:

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
Você também pode agendar essa ferramenta para execução periódica usando uma tarefa agendada. Para saber mais sobre como usar o VSS, consulte [Vssadmin](../../administration/windows-commands/vssadmin.md). O backup de volumes de log não é necessário nem tem valor. Tentativas de fazer isso serão ignoradas pelo VSS.
O uso do Backup do Windows Server, do Backup do Microsoft Azure, do Microsoft DPM ou de outras tecnologias de instantâneos, VSS, máquina virtual ou baseadas em arquivo têm suporte na Réplica de Armazenamento contanto que operem na camada do volume. A Réplica de Armazenamento não dá suporte ao backup e à restauração baseados em bloco.

## <a name="FAQ14"></a> Pode configurar a replicação para restringir o uso de largura de banda?
Sim, por meio do limitador de largura de banda do SMB. Essa é uma configuração global para todo o tráfego de Réplica de Armazenamento e, assim, afeta toda a replicação desse servidor. Normalmente, isso é necessário apenas com a configuração de sincronização inicial da Réplica de Armazenamento, em que todos os dados do volume devem ser transferidos. Se for necessário após a sincronização inicial, a largura de banda de rede será muito baixa para sua carga de trabalho de E/S. Reduza a E/S ou aumente a largura de banda.

Isso só deve ser usado com a replicação assíncrona (observação: a sincronização inicial é sempre assíncrona, mesmo que você tenha especificado que ela seja síncrona).
Você também pode usar políticas de QoS de rede para moldar o tráfego de Réplica de Armazenamento. O uso da replicação de Réplica de Armazenamento com propagação altamente correspondente também reduzirá consideravelmente o uso de largura de banda de sincronização inicial geral.


Para definir o limite de largura de banda, use:

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

Para ver o limite de largura de banda, use:

    Get-SmbBandwidthLimit -Category StorageReplication

Para remover o limite de largura de banda, use:

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>Quais portas de rede que requer a réplica de armazenamento?
A Réplica de Armazenamento depende de SMB e WSMAN para replicação e gerenciamento. Isso significa que as seguintes portas são necessárias:

 445 (SMB - protocolo de transporte de replicação, o protocolo de gerenciamento de RPC de cluster) 5445 (SMB - só é necessário ao usar a rede RDMA iWARP iWARP) 5985 (WSManHTTP - protocolo de gerenciamento do CIM/WMI/PowerShell)

Observação: O cmdlet Test-SRTopology requer ICMPv4/ICMPv6, mas não para replicação ou gerenciamento.

## <a name="FAQ15.5"></a>Quais são as práticas recomendadas do volume de log?
O tamanho ideal do log varia amplamente por ambiente e a carga de trabalho e é determinado pela quantidade executa sua carga de trabalho de e/s de gravação. 

1.  Um log maior ou menor não faz você qualquer mais rápida ou mais lenta
2.  Um log maior ou menor não tem qualquer que ostentam em um volume de dados de 10GB versus um volume de dados de 10TB, por exemplo

Um log maior simplesmente coleta e retém gravação mais IOs antes que eles são encapsulados-out. Isso permite que uma interrupção no serviço entre o computador de origem e destino – como uma interrupção na rede ou de destino sendo offline - para ir mais tempo. Se o log palpáveis 10 horas de gravações, e a rede cai para 2 horas, quando a rede retorna que a origem pode simplesmente reproduzir o delta das alterações de volta para o destino muito rápido e você está protegido novamente muito rapidamente. Se o log contém 10 horas e a interrupção é 2 dias, a fonte agora tem a reprodução de um log de diferente chamado o bitmap – e provavelmente será mais lenta entrar novamente em sincronizar. Depois que em sincronia retorna para usar o log.

A Réplica de Armazenamento depende do log para todo o desempenho da gravação. Desempenho de log essencial para o desempenho de replicação. Certifique-se de que o volume de log tem melhor desempenho que o volume de dados, como o log será serializar e sequentialize todos e/s de gravação. Você sempre deve usar uma mídia flash, como SSD, em volumes de log. Você nunca deve permitir que outras cargas de trabalho sejam executadas no volume do log. Da mesma maneira, você nunca deve permitir que outras cargas de trabalho sejam executadas em volumes de log do banco de dados SQL. 

Novamente: É altamente recomendável que o armazenamento de log seja mais rápido do que o armazenamento de dados e volumes de log nunca devem ser usados para outras cargas de trabalho.

Você pode obter recomendações de dimensionamento de log executando a ferramenta de Test-SRTopology. Como alternativa, você pode usar contadores de desempenho em servidores existentes para fazer um julgamento de tamanho do log. A fórmula é simple: monitorar a taxa de disco de dados (médio de Bytes de gravação/s) sob a carga de trabalho e usá-la para calcular a quantidade de tempo que levará para preencher o log de tamanhos diferentes. Por exemplo, taxa de transferência de disco de dados de 50 MB/s fará com que o log de 120GB para encapsular em 40 minutos, 2400 segundos ou segundos de 120GB / 50MB. Portanto, a quantidade de tempo que o servidor de destino pode estar inacessível antes do logon encapsulado é de 40 minutos. Se o log encapsula, mas o destino se torna acessível novamente, a fonte seria reproduzir blocos por meio do log de mapa de bits em vez do log principal. O tamanho do log não tem um efeito no desempenho.

SOMENTE o disco de dados do cluster de origem deve ser armazenado em backup. Os discos de Log da réplica de armazenamento devem não ser backup, pois um backup pode entrar em conflito com as operações de réplica de armazenamento.

## <a name="FAQ16"></a> Por que você escolheria um cluster estendido em comparação com o cluster para cluster em comparação com a topologia de servidor para servidor?  
A réplica de armazenamento é fornecido em três configurações principais: Alongar cluster, o cluster para cluster e o servidor para servidor. Há várias vantagens a cada um.

A topologia de cluster estendido é ideal para exigir que o failover automático com coordenação, como clusters de nuvem privada do Hyper-V e SQL Server FCI as cargas de trabalho. Ele também tem uma interface gráfica interna usando o Gerenciador de Cluster de Failover. Ele utiliza o clássico assimétrico arquitetura de armazenamento compartilhado de espaços de armazenamento, SAN, iSCSI, do cluster e RAID via reserva persistente. Você pode executar isso com um mínimo de 2 nós.

A topologia de cluster ao cluster usa dois clusters separados e é ideal para os administradores que desejam failover manual, especialmente quando o segundo local é provisionado para uso não diário e recuperação de desastres. Coordenação é manual. Ao contrário do cluster estendido, espaços de armazenamento diretos pode ser usado nessa configuração (com advertências - consulte a perguntas frequentes sobre a réplica de armazenamento e a documentação de cluster para cluster). Você pode executar isso com um mínimo de quatro nós. 

A topologia de servidor a servidor é ideal para clientes que executam o hardware que não pode ser agrupado. Isso requer coordenação e failover manual. Ele é ideal para implantações de baixo custo entre filiais e data centers centrais, especialmente ao usar a replicação assíncrona. Essa configuração geralmente pode substituir a instâncias de servidores de arquivos protegidos DFSR usado para cenários de recuperação de desastres de mestre único.

Em todos os casos, as topologias suportam a ambas as em execução no hardware físico, bem como em máquinas virtuais. Quando estiver em máquinas virtuais, o hipervisor subjacente não exige o Hyper-V; ele pode ser VMware, KVM, Xen, etc.

Armazenamento réplica também tem um modo de servidor-para-self, onde você apontar replicação em dois volumes diferentes no mesmo computador.

## <a name="FAQ18"></a> Eliminação de duplicação de dados é compatível com a réplica de armazenamento?

Sim, os dados Deduplcation é compatível com a réplica de armazenamento. Habilitar a eliminação de duplicação de dados em um volume no servidor de origem e, durante a replicação, o servidor de destino recebe uma cópia com eliminação de duplicação do volume.

Enquanto você deve *instale* eliminação de duplicação de dados em servidores de origem e de destino (consulte [instalando e eliminação de duplicação de dados](../data-deduplication/install-enable.md)), ele é importante não *habilitar*Eliminação de duplicação de dados no servidor de destino. A réplica de armazenamento permite que as gravações somente no servidor de origem. Porque a eliminação de duplicação de dados torna as gravações no volume, ele deve ser executado somente no servidor de origem. 

## <a name="FAQ19"></a> É possível replicar entre 2019 do Windows Server e Windows Server 2016?

Infelizmente, não damos suporte a criação de um *novo* parceria entre 2019 do Windows Server e Windows Server 2016. Você pode atualizar com segurança um servidor ou cluster que executa o Windows Server 2016 para Windows Server 2019 e qualquer *existentes* parcerias continuarão a funcionar.

No entanto, para obter o melhor desempenho do Windows Server 2019, todos os membros da parceria devem executar o Windows Server 2019 e você deve excluir parcerias existentes e associadas a grupos de replicação e, em seguida, recriá-los com os dados propagados (seja ao criar a parceria no Windows Admin Center ou com o cmdlet New-SRPartnership).

## <a name="FAQ17"></a> Como faço para relatar um problema com a réplica de armazenamento ou este guia?  
Para obter assistência técnica para a Réplica de Armazenamento, poste nos [fóruns do Microsoft TechNet](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview). Você também pode enviar um email a srfeed@microsoft.com perguntas sobre a Réplica de Armazenamento ou problemas com esta documentação. O <https://windowsserver.uservoice.com> site é preferencial para solicitações de alteração de design, pois permite que seus clientes fornecer suporte e comentários sobre suas ideias.



## <a name="related-topics"></a>Tópicos relacionados  
- [Visão geral da réplica de armazenamento](storage-replica-overview.md) 
- [Replicação de Cluster estendido usando armazenamento compartilhado](stretch-cluster-replication-using-shared-storage.md)  
- [Replicação de armazenamento de servidor para servidor](server-to-server-storage-replication.md)  
- [Replicação de armazenamento de Cluster para cluster](cluster-to-cluster-storage-replication.md)  
- [Réplica de armazenamento: Problemas conhecidos](storage-replica-known-issues.md)  

## <a name="see-also"></a>Consulte também  
- [Visão geral de armazenamento](../storage.md)  
- [Espaços de Armazenamento Diretos](../storage-spaces/storage-spaces-direct-overview.md)  
