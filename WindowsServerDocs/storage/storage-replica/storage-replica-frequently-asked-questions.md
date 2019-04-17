---
title: "Perguntas frequentes sobre Réplica de Armazenamento"
ms.prod: windows-server-threshold
manager: siroy
ms.author: nedpyle
ms.technology: storage-replica
ms.topic: get-started-article
author: nedpyle
ms.date: 07/20/2017
ms.assetid: 12bc8e11-d63c-4aef-8129-f92324b2bf1b
ms.openlocfilehash: f29ca24a39e00f5142fe700e6abb09d1673acf7b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="frequently-asked-questions-about-storage-replica"></a>Perguntas frequentes sobre Réplica de Armazenamento

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Este tópico contém respostas às perguntas frequentes sobre Réplica de Armazenamento.

## <a name="FAQ1"></a> Há suporte à Réplica de Armazenamento no Nano Server?  
Sim.  

> [!NOTE]
> Você deve usar o pacote **Armazenamento** do Nano Server durante a instalação. Para saber mais sobre como implantar o Nano Server, consulte [Introdução ao Nano Server](https://technet.microsoft.com/library/mt126167.aspx).  

Instale a Réplica de Armazenamento no Nano Server usando a comunicação remota do PowerShell da seguinte maneira:  

1. Adicione o Nano Server à sua lista de clientes confiáveis.  
   > [!NOTE]
   > Essa etapa só será necessária se o computador não for membro de uma floresta de Active Directory Domain Services ou se estiver em uma floresta não confiável. Ele adiciona suporte NTLM para comunicação remota de PSSession, que é desabilitado por padrão por motivos de segurança. Para saber mais, consulte [Considerações de segurança de comunicação remota do PowerShell](https://msdn.microsoft.com/powershell/scriptiwinrmsecurity).  

   ```  
       Set-Item WSMan:\localhost\Client\TrustedHosts "<computer name of Nano Server>"  
   ```  
2.  Para instalar o recurso de Réplica de Armazenamento, execute o seguinte cmdlet em um computador de gerenciamento:  

    ```  
    Install-windowsfeature -Name storage-replica,RSAT-Storage-Replica -ComputerName <nano server> -Restart -IncludeManagementTools  
    ```  

    Usando o cmdlet `Test-SRTopology` com o Nano Server no Windows Server 2016 requer a chamada de um script remoto com CredSSP. Diferentemente de outros cmdlets de Réplica de Armazenamento, `Test-SRTopology` requer execução local no servidor de origem.   
No Nano Server (por meio de um PSSession remoto):  

    >[!NOTE]
    >O CREDSSP é necessário para oferecer suporte de salto duplo para Kerberos no cmdlet `Test-SRTopology` e não é necessário por outros cmdlets de Réplica de Armazenamento, que lidam com credenciais de sistema distribuídas automaticamente. Não é recomendável usar CREDSSP em circunstâncias normais. Para obter uma alternativa ao CREDSSP, analise a seguinte postagem do blog da Microsoft: "Comunicação remota de salto duplo de Kerberos com PowerShell solucionado com segurança" - https://blogs.technet.microsoft.com/ashleymcglone/2016/08/30/powershell-remoting-kerberos-double-hop-solved-securely/ 

        Enable-WSManCredSSP -role server       

    No computador de gerenciamento:  

         Enable-WSManCredSSP Client -DelegateComputer <remote server name>  

         $CustomCred = Get-Credential  

         Invoke-Command -ComputerName sr-srv01 -ScriptBlock { Test-SRTopology <commands> } -Authentication Credssp -Credential $CustomCred  

       Em seguida, copie os resultados para o computador de gerenciamento ou compartilhe o caminho. Como o Nano não tem as bibliotecas gráficas necessárias, você pode usar Test-SRTopology para processar os resultados e obter um arquivo de relatório com gráficos. Por exemplo:  

        Test-SRTopology -GenerateReport -DataPath \\sr-srv05\c$\temp  


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

## <a name="FAQ3"></a>Posso determinar adaptadores de rede específicas a serem usadas para replicação?  

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

## <a name="FAQ4"></a> Posso configurar a replicação de um para muitos ou a replicação transitiva (A para B para C)?  
Não no Windows Server 2016. Esta versão só oferece suporte à replicação de um para um de um servidor, cluster ou nó de cluster estendido. Isso pode mudar em uma versão posterior. Você pode, claro, configurar a replicação entre vários servidores de um par de volumes específico, em qualquer direção. Por exemplo, o Servidor 1 pode replicar seu volume D no Servidor 2, e seu volume E do Servidor 3.

## <a name="FAQ5"></a> Posso aumentar ou reduzir os volumes replicados pela Réplica de Armazenamento?  
Você pode aumentar (expandir) os volumes, mas não pode reduzi-los. Por padrão, a Réplica de Armazenamento impede que os administradores estendam os volumes replicados; use a opção `Set-SRGroup -AllowVolumeResize $TRUE` no grupo de origem, antes do redimensionamento. Por exemplo:

1. Uso contra o computador de origem: `Set-SRGroup -Name YourRG -AllowVolumeResize $TRUE`
2. Aumentar o volume usando qualquer técnica de sua preferência
3. Uso contra o computador de origem: `Set-SRGroup -Name YourRG -AllowVolumeResize $FALSE` 

## <a name="FAQ6"></a>Posso colocar online um volume de destino para acesso somente leitura?  
Não no Windows Server 2016 RTM, também conhecido como versão "RS1". Armazenamento réplica desmonta o volume de destino quando a replicação começa. 

No entanto, na versão do Windows Server, 1709, a opção para montar o armazenamento de destino agora é possível. Esse recurso é chamado de "Teste de Failover". Para fazer isso, você deve ter um volume não utilizado, com formatação NTFS ou ReFS, que não está atualmente replicando no destino. Em seguida, você pode montar um instantâneo do armazenamento replicado em nós de destino temporariamente para fins de teste ou backup. 

Por exemplo, para criar um failover de teste onde você replica um volume "D:" no Grupo de replicação "RG2" do servidor de destino "SRV2" e tem uma unidade "T:" em SRV2 que não está sendo duplicada:

 `Mount-SRDestination -Name RG2 -Computername SRV2 -TemporaryPath T:\`
 
O volume replicado D: agora é acessível em SRV2. Você pode ler e gravar o volume normalmente, copiar arquivos dele ou executar um backup online que você salva em outro lugar para segurança.

Para remover o instantâneo de failover de teste e descartar suas alterações:

 `Dismount-SRDestination -Name RG2 -Computername SRV2`
 
Você deve usar somente o recurso de failover de teste para operações temporárias em curto prazo. Ele não se destina ao uso de longo prazo. Quando estiver em uso, a replicação continua no volume de destino real. 

## <a name="FAQ7"></a> É possível configurar o SOFS (Servidor de Arquivos de Escalabilidade Horizontal) em um cluster estendido?  
Embora seja tecnicamente possível, essa não é uma configuração recomendada no Windows Server 2016 devido à falta de reconhecimento de local nos nós de computação que contatam o SOFS. Se for usada a rede de distância de campus, em que as latências são, normalmente, inferiores a milissegundos, essa configuração provavelmente funcionará sem problemas.   

Se for configurada a replicação de cluster para cluster, a Réplica de Armazenamento dará suporte completo a Servidores de Arquivos de Escalabilidade Horizontal, incluindo o uso de Espaços de Armazenamento Direto, ao replicar entre dois clusters.  

## <a name="FAQ8"></a>Posso configurar Espaços de Armazenamento Direto em um cluster estendido com a Réplica de Armazenamento?  
Essa não é uma configuração compatível com o Windows Server 2016.  Isso pode mudar em uma versão posterior. Se for configurada a replicação de cluster para cluster, a Réplica de Armazenamento dará suporte completo a Servidores de Arquivos de Escalabilidade Horizontal e Servidores Hyper-V, incluindo o uso de Espaços de Armazenamento Direto.  

## <a name="FAQ9"></a>Como configurar a replicação assíncrona?  

Especifique `New-SRPartnership -ReplicationMode` e forneça o argumento **Asynchronous**. Por padrão, toda replicação na Réplica de Armazenamento é síncrona. Você também pode alterar o modo com `Set-SRPartnership -ReplicationMode`.  

## <a name="FAQ10"></a>Como impedir o failover automático de um cluster estendido?  
Para evitar o failover automático, você pode usar o PowerShell para configurar `Get-ClusterNode -Name "NodeName").NodeWeight=0`. Isso remove o voto em cada nó no local de recuperação de desastre. Você pode usar `Start-ClusterNode -PreventQuorum` em nós no local principal e `Start-ClusterNode -ForceQuorum` em nós no local de desastre para forçar o failover. Não há uma opção gráfica para evitar o failover automático, não é recomendado e impedir o failover automático.  

## <a name="FAQ11"></a>Como desabilitar a resiliência de máquina virtual?  
Para impedir que o novo recurso de resiliência de máquina virtual de Hyper-V seja executado e, portanto, pause máquinas virtuais em vez de fazer o failover delas para o local de recuperação de desastre, execute `(Get-Cluster).ResiliencyDefaultPeriod=0`  

## <a name="FAQ12"></a> Como reduzir o tempo de sincronização inicial?  

Você pode usar o armazenamento provisionado como uma maneira de agilizar os tempos de sincronização inicial. A Réplica de Armazenamento consulta e usa automaticamente o armazenamento provisionado dinâmico, incluindo Espaços de Armazenamento sem clusters, discos dinâmicos Hyper-V e LUNs SAN.  

Também é possível usar volumes de dados propagados para reduzir a largura de banda e, algumas vezes, economizar tempo, ao garantir que o volume de destino tenha algum subconjunto dos dados do principal (por meio de um backup restaurado, instantâneo antigo, replicação anterior, arquivos copiados, etc.) e, então, usando a opção Propagado no Gerenciador de Cluster de Failover ou em `New-SRPartnership`. Se o volume estiver quase vazio, usar a sincronização propagada pode reduzir o uso de largura de banda e economizar tempo.

## <a name="FAQ13"></a> Posso delegar usuários para administrar a replicação?  

Você pode usar o cmdlet `Grant-SRDelegation` no Windows Server 2016. Isso permite que você defina usuários específicos em cenários de replicação de servidor para servidor, cluster para cluster e cluster estendido com as permissões para criar, alterar ou remover a replicação, sem serem membros do grupo de administradores locais. Por exemplo:  

    Grant-SRDelegation -UserName contso\tonywang  

O cmdlet o lembrará de que o usuário precisa fazer logoff e logon do servidor que pretende administrar para que a alteração tenha efeito. Você pode usar `Get-SRDelegation` e `Revoke-SRDelegation` para controlar isso.  

## <a name="FAQ13"></a> Quais são as opções de backup e restauração para volumes replicados?
A Réplica de Armazenamento oferece suporte a backup e restauração do volume de origem. Também à criação e restauração de instantâneos do volume de origem. Você não pode fazer backup nem restaurar o volume de destino enquanto ele é protegido pela Réplica de Armazenamento, pois ele não está montado nem acessível. Se ocorrer um desastre em que o volume de origem seja perdido, o uso de `Set-SRPartnership` para promover o volume de destino anterior para ser, agora, uma origem de leitura/gravação permitirá que você faça backup ou restaure o volume. Também é possível remover a replicação com `Remove-SRPartnership` e `Remove-SRGroup` para remontar o volume como leitura/gravação.
Para criar instantâneos de aplicativos consistentes e periódicos, você pode usar VSSADMIN.EXE no servidor de origem para criar instantâneos de volumes de dados replicados. Por exemplo, você está replicando o volume F: com a Réplica de Armazenamento:

    vssadmin create shadow /for=F:
Em seguida, depois de alternar a direção da replicação, remover a replicação ou se simplesmente ainda estiver no mesmo volume de origem, é possível restaurar qualquer instantâneo para seu ponto no tempo. Por exemplo, ainda usando F:

    vssadmin list shadows
     vssadmin revert shadow /shadow={shadown copy ID GUID listed previously}
Você também pode agendar essa ferramenta para execução periódica usando uma tarefa agendada. Para saber mais sobre como usar o VSS, consulte [Vssadmin](https://technet.microsoft.com/library/cc754968.aspx). O backup de volumes de log não é necessário nem tem valor. Tentativas de fazer isso serão ignoradas pelo VSS.
O uso do Backup do Windows Server, do Backup do Microsoft Azure, do Microsoft DPM ou de outras tecnologias de instantâneos, VSS, máquina virtual ou baseadas em arquivo têm suporte na Réplica de Armazenamento contanto que operem na camada do volume. A Réplica de Armazenamento não dá suporte ao backup e à restauração baseados em bloco.

## <a name="FAQ14"></a> Posso configurar a replicação para restringir o uso de largura de banda?
Sim, por meio do limitador de largura de banda do SMB. Essa é uma configuração global para todo o tráfego de Réplica de Armazenamento e, assim, afeta toda a replicação desse servidor. Normalmente, isso é necessário apenas com a configuração de sincronização inicial da Réplica de Armazenamento, em que todos os dados do volume devem ser transferidos. Se for necessário após a sincronização inicial, a largura de banda de rede será muito baixa para sua carga de trabalho de E/S. Reduza a E/S ou aumente a largura de banda.

Isso só deve ser usado com a replicação assíncrona (observação: a sincronização inicial é sempre assíncrona, mesmo que você tenha especificado que ela seja síncrona).
Você também pode usar políticas de QoS de rede para moldar o tráfego de Réplica de Armazenamento. O uso da replicação de Réplica de Armazenamento com propagação altamente correspondente também reduzirá consideravelmente o uso de largura de banda de sincronização inicial geral.


Para definir o limite de largura de banda, use:

    Set-SmbBandwidthLimit  -Category StorageReplication -BytesPerSecond x

Para ver o limite de largura de banda, use:

    Get-SmbBandwidthLimit -Category StorageReplication

Para remover o limite de largura de banda, use:

    Remove-SmbBandwidthLimit -Category StorageReplication
    
## <a name="FAQ15"></a>Quais portas de rede são exigidas pela Réplica de Armazenamento?
A Réplica de Armazenamento depende de SMB e WSMAN para replicação e gerenciamento. Isso significa que as seguintes portas são necessárias:

   445 (SMB - protocolo de transporte de replicação) 5445 (iWARP SMB - necessário somente ao usar rede de iWARP RDMA) 5895 (WSManHTTP - protocolo de gerenciamento para WMI/CIM/PowerShell)

Observação: O cmdlet Test-SRTopology requer ICMPv4/ICMPv6, mas não para replicação nem gerenciamento.

## <a name="FAQ15"></a>Quais são as práticas recomendadas de volume do log?
O tamanho ideal de tamanho do log varia muito por ambiente e da carga de trabalho e é determinado pelo quanto escrever sua carga de trabalho executa de e/s. 

1.  Um log maior ou menor não faz você qualquer mais rápida ou mais lenta
2.  Um log maior ou menor não tem qualquer que ostentam em um volume de dados de 10GB versus um volume de dados de 10TB, por exemplo

Um log maior simplesmente coleta e retém mais gravações IOs antes de serem encapsulados. Isso permite que uma interrupção no serviço entre o computador de origem e destino – como uma interrupção na rede ou de destino sendo offline - dure mais tempo. Se o log palpáveis 10 horas de gravações, e a rede cai para 2 horas, quando a rede retorna que a origem pode simplesmente reproduzir o delta das alterações de volta para o destino muito rápido e você está protegido novamente muito rapidamente. Se o log contém 10 horas e a interrupção é 2 dias, a fonte agora tem a reprodução de um log diferente chamado o bitmap – e provavelmente será mais lento para entrar novamente em sincronização. Depois que estiver em sincronia, ele volta a usar o log.

Há contadores de desempenho de SR que informará a taxa que está processando o log, permitindo que você faça algumas judgements. Também o cmdlet Test-SRTopology faz isso. Basicamente você está apenas olhando e/s de gravação em uma carga de trabalho existente para decidir quanto IO provavelmente irá fluir o log por minuto, horas ou dias.

A Réplica de Armazenamento depende do log para todo o desempenho da gravação. Desempenho de log essencial para o desempenho de replicação. Certifique-se de que o volume de log tem melhor desempenho que o volume de dados, como o log será serializar e sequentialize todos e/s de gravação. Você sempre deve usar uma mídia flash, como SSD, em volumes de log. Você nunca deve permitir que outras cargas de trabalho sejam executadas no volume do log. Da mesma maneira, você nunca deve permitir que outras cargas de trabalho sejam executadas em volumes de log do banco de dados SQL. 

Novamente: Microsoft recomenda veementemente que o armazenamento de log seja mais rápido do que o armazenamento de dados e volumes de log nunca devem ser usados para outras cargas de trabalho.

## <a name="FAQ16"></a> Por que você escolha um cluster estendido versus cluster ao cluster versus topologia de servidor a servidor?  
Armazenamento réplica vem em três configurações principais: strech cluster, cluster ao cluster e servidor a servidor. Há várias vantagens a cada um.

A topologia de cluster estendido é ideal para exigir que o failover automático com coordenação, como clusters de nuvem privada do Hyper-V e SQL Server FCI as cargas de trabalho. Ele também tem uma interface gráfica interna usando o Gerenciador de Cluster de Failover. Ele utiliza o clássico assimétrico arquitetura de armazenamento compartilhado de espaços de armazenamento, SAN, iSCSI, do cluster e RAID via reserva persistente. Você pode executar isso com um mínimo de 2 nós.

A topologia de cluster ao cluster usa dois clusters separados e é ideal para os administradores que desejam failover manual, especialmente quando o segundo local é provisionado para uso não diário e recuperação de desastres. Coordenação é manual. Ao contrário de cluster estendido, direto de espaços de armazenamento pode ser usado nessa configuração. Você pode executar isso com um mínimo de quatro nós. 

A topologia de servidor a servidor é ideal para clientes que executam o hardware que não pode ser agrupado. Isso requer coordenação e failover manual. Ele é ideal para implantações baratas entre filiais e centros de dados centrais, especialmente ao usar replicação assíncrona. Essa configuração geralmente pode substituir a instâncias de servidores de arquivos protegidos DFSR usado para cenários de recuperação de desastres de mestre único.

Em todos os casos, as topologias suportam a ambas as em execução no hardware físico, bem como em máquinas virtuais. Quando estiver em máquinas virtuais, o hipervisor subjacente não exige o Hyper-V; ele pode ser VMware, KVM, Xen, etc.

Armazenamento réplica também tem um modo de servidor-para-self, onde você apontar replicação em dois volumes diferentes no mesmo computador.

## <a name="FAQ17"></a> Como relatar um problema com este guia ou com a Réplica de Armazenamento?  
Para obter assistência técnica para a Réplica de Armazenamento, poste nos [fóruns do Microsoft TechNet](https://social.technet.microsoft.com/Forums/windowsserver/en-US/home?forum=WinServerPreview). Você também pode enviar um email a srfeed@microsoft.com perguntas sobre a Réplica de Armazenamento ou problemas com esta documentação. O site https://windowsserver.uservoice.com é preferencial para solicitações de alteração de design, pois permite que seus clientes forneçam suporte e comentários sobre suas ideias.



## <a name="related-topics"></a>Tópicos relacionados  
- [Visão geral da Réplica de Armazenamento](storage-replica-overview.md) 
- [Replicação de cluster estendido usando Armazenamento Compartilhado](stretch-cluster-replication-using-shared-storage.md)  
- [Replicação de armazenamento de servidor para servidor](server-to-server-storage-replication.md)  
- [Replicação de armazenamento de cluster para cluster](cluster-to-cluster-storage-replication.md)  
- [Réplica de Armazenamento: problemas conhecidos](storage-replica-known-issues.md)  

## <a name="see-also"></a>Consulte Também  
- [Visão geral de armazenamento](../storage.md)  
- [Espaços de Armazenamento Diretos no Windows Server 2016](../storage-spaces/storage-spaces-direct-overview.md)  
