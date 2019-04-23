---
title: O que há de novo no Hyper-V no Windows Server 2016
description: Fornece um resumo dos novos recursos no Hyper-V
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
author: KBDAzure
ms.author: kathydav
ms.date: 09/21/2017
ms.openlocfilehash: 1384a15d3b8ecee32d36c6265d478edace0bdb09
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848097"
---
# <a name="whats-new-in-hyper-v-on-windows-server-2016"></a>O que há de novo no Hyper-V no Windows Server 2016

>Aplica-se a: Microsoft Hyper-V Server 2016, Windows Server 2016
  
Este artigo explica as funcionalidades novas e alteradas do Hyper-V no Windows Server 2016 e o Microsoft Hyper-V Server 2016. Para usar os novos recursos em máquinas virtuais criadas com o Windows Server 2012 R2 e movida ou importado em um servidor que executa o Hyper-V no Windows Server 2016, você precisará atualizar manualmente a versão de configuração de máquina virtual. Para obter instruções, consulte [máquinas virtuais de atualização de versão](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).  
  
Aqui está o que está incluído neste artigo e se a funcionalidade é nova ou atualizada.  

## <a name="BKMK_standby"></a>Compatível com o modo de espera conectado \(novo\)
Quando a função Hyper-V é instalada em um computador que usa o modelo de energia AOAC sempre ativo / conectado (), o **modo de espera conectado** estado de energia agora está disponível.  
  
## <a name="BKMK_device"></a>Atribuição de dispositivo discretos \(novo\) 
Esse recurso permite que você fornecer um acesso direto e exclusivos de máquina virtual para alguns dispositivos de hardware PCIe. Usando um dispositivo dessa maneira ignora a pilha de virtualização Hyper-V, que resulta em um acesso mais rápido. Para obter detalhes sobre o hardware com suporte, consulte "atribuição de dispositivo discretos" na [requisitos de sistema do Hyper-V no Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Para obter detalhes, incluindo como usar esse recurso e considerações, consulte a postagem "[atribuição de dispositivo discretos — descrição e o plano de fundo](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)" no blog de virtualização.

## <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>Suporte à criptografia para o disco do sistema operacional em máquinas virtuais de geração 1 \(novo)

Agora você pode proteger o disco do sistema operacional usando a criptografia de unidade de disco BitLocker em máquinas virtuais de geração 1. Um novo recurso, o armazenamento de chaves, cria uma unidade pequena e dedicada para armazenar a chave do BitLocker da unidade do sistema. Isso é feito em vez de usar um virtual Trusted Platform Module (TPM), que está disponível somente em máquinas virtuais de geração 2. Para descriptografar o disco e iniciar a máquina virtual, o host Hyper-V deve ser parte de uma malha protegida autorizada ou tem a chave privada de um dos guardiões da máquina virtual. Armazenamento de chaves requer uma máquina de virtual versão 8. Para obter informações sobre a versão da máquina virtual, consulte [máquinas virtuais de atualização de versão no Hyper-V no Windows 10 ou Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).  
  
## <a name="BKMK_host"></a>Proteção de recursos de host \(novo\)
Esse recurso ajuda a impedir que uma máquina virtual usando mais do que o seu compartilhamento de recursos do sistema procurando por níveis de excesso de atividade. Isso pode ajudar a impedir que uma atividade excessiva da máquina virtual degradar o desempenho do host ou de outras máquinas virtuais. Quando o monitoramento detecta uma máquina virtual com uma atividade excessiva, a máquina virtual recebe menos recursos. Esse monitoramento e a imposição está desativado por padrão. Use o Windows PowerShell para ativar ou desativar a ele. Para ativá-lo, execute este comando:  
  
```  
Set-VMProcessor TestVM -EnableHostResourceProtection $true 
```       

Para obter detalhes sobre esse cmdlet, consulte [Set-VMProcessor](https://docs.microsoft.com/powershell/module/hyper-v/set-vmprocessor).

## <a name="BKMK_hot"></a>Adição e remoção de adaptadores de rede e memória ativa \(novo\) 
Agora você pode adicionar ou remover um adaptador de rede enquanto a máquina virtual está em execução, sem incorrer em tempo de inatividade. Isso funciona para máquinas virtuais de geração 2 que executam sistemas operacionais Windows ou Linux.  
  
Você também pode ajustar a quantidade de memória atribuída a uma máquina virtual enquanto estiver em execução, mesmo se você não tiver habilitado a memória dinâmica. Isso funciona para a geração 1 e máquinas virtuais de geração 2, executando o Windows Server 2016 ou Windows 10.  
  
## <a name="BKMK_Mgmt"></a>Melhorias do Gerenciador do Hyper-V \(atualizado\) 
  
-   **Suporte a credenciais alternativas** -agora você pode usar um conjunto diferente de credenciais no Gerenciador do Hyper-V quando você se conecta a outro host remoto do Windows Server 2016 ou Windows 10. Você também pode salvar essas credenciais para torná-lo mais fácil de fazer logon novamente.  
  
-   **Gerenciar versões anteriores** -com o Gerenciador de Hyper-V no Windows Server 2016 e Windows 10, você pode gerenciar computadores que executam o Hyper-V no Windows Server 2012, Windows 8, Windows Server 2012 R2 e Windows 8.1.  
  
-   **Protocolo de gerenciamento atualizado** -Gerenciador Hyper-V agora se comunica com hosts remotos do Hyper-V usando o protocolo WS-MAN, que permite que a autenticação CredSSP, Kerberos ou NTLM. Quando você usa o CredSSP para se conectar a um host remoto do Hyper-V, você pode fazer uma migração ao vivo sem habilitar a delegação restrita no Active Directory. A infraestrutura baseados em WS-MAN também torna mais fácil habilitar um host para o gerenciamento remoto. O WS-MAN se conecta pela porta 80, que é aberta por padrão.  
  
## <a name="BKMK_IS"></a>Serviços de integração fornecidos por meio do Windows Update \(atualizado\) 
Atualizações de serviços de integração para convidados do Windows são distribuídas por meio do Windows Update. Para provedores de serviço e aos hosters em nuvem privada, isso faz com que o controle de aplicação de atualizações nas mãos de locatários que possuem as máquinas virtuais. Locatários podem agora atualizar suas máquinas de virtuais do Windows com todas as atualizações, incluindo os serviços de integração, usando um único método. Para obter detalhes sobre os serviços de integração para convidados do Linux, consulte [FreeBSD máquinas virtuais Linux e no Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).  
  
> [!IMPORTANT]  
> O arquivo de imagem de vmguest não é necessário, para que ele não está incluído com o Hyper-V no Windows Server 2016.  
  
## <a name="BKMK_linux"></a>Inicialização segura do Linux \(novo\) 
Sistemas operacionais Linux em máquinas virtuais de geração 2 agora podem ser inicializados com a opção de inicialização segura habilitada. Ubuntu 14.04 e versões posterior, SUSE Linux Enterprise Server 12 e posterior, o Red Hat Enterprise Linux 7.0 e posterior e CentOS 7.0 e versões posteriores estão habilitados para inicialização segura em hosts que executam o Windows Server 2016. Antes de inicializar a máquina virtual pela primeira vez, você deve configurar a máquina virtual para usar a autoridade de certificação Microsoft UEFI. Você pode fazer isso do Gerenciador do Hyper-V, Virtual Machine Manager ou uma sessão do Windows Powershell com privilégios elevados. Para o Windows PowerShell, execute este comando:  
  
```  
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority  
```  
  
Para obter mais informações sobre as máquinas virtuais Linux no Hyper-V, consulte [FreeBSD máquinas virtuais Linux e no Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md). Para obter mais informações sobre o cmdlet, consulte [Set-VMFirmware](https://docs.microsoft.com/powershell/module/hyper-v/set-vmfirmware).

## <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>Mais memória e processadores para máquinas virtuais de geração 2 e hosts Hyper-V \(atualizado\)
Começando com a versão 8, as máquinas virtuais de geração 2 pode usar significativamente mais memória e processadores virtuais. Hosts também podem ser configurados com significativamente mais memória e processadores virtuais que anteriormente tinham suporte. Essas alterações dão suporte a novos cenários como a execução de comércio eletrônico grandes bancos de dados na memória para processamento de transações online (OLTP) e data warehousing (DW). Blog do Windows Server publicadas recentemente os resultados de desempenho de uma máquina virtual com 5.5 terabytes de memória e 128 processadores virtuais executando o banco de dados do 4 TB de memória. Desempenho era maior que 95% do desempenho de um servidor físico. Para obter detalhes, consulte [desempenho em larga escala de VM do Windows Server 2016 Hyper-V para o processamento de transações na memória](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Para obter detalhes sobre as versões de máquina virtual, consulte [máquinas virtuais de atualização de versão no Hyper-V no Windows 10 ou Windows Server 2016](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obter a lista completa de configurações de máximo com suporte, consulte [planejar a escalabilidade do Hyper-V no Windows Server 2016](./plan/plan-hyper-v-scalability-in-windows-server.md). 

## <a name="BKMK_nested"></a>A virtualização aninhada \(novo\) 
Esse recurso permite usar uma máquina virtual como um host Hyper-V e criar máquinas virtuais no host virtualizado. Isso pode ser especialmente útil para ambientes de desenvolvimento e teste. Para usar a virtualização aninhada, você precisará de:  
  
-   Para executar pelo menos Windows Server 2016 ou Windows 10 no host físico do Hyper-V e o host virtualizado.  
  
-   Um processador com Intel VT-x (virtualização aninhada está disponível somente para os processadores Intel neste momento).  
  
Para obter detalhes e instruções, consulte [executar Hyper-V em uma máquina Virtual com a virtualização aninhada](https://docs.microsoft.com/virtualization/hyper-v-on-windows/user-guide/nested-virtualization).  
  
## <a name="BKMK_networking"></a>Recursos de rede \(novo\) 
Novos recursos de rede incluem:  
  
-   **Remote direcionar o acesso à memória (RDMA) e switch embedded agrupamento (SET)**. Você pode configurar o RDMA nos adaptadores de rede associados a um comutador virtual Hyper-V, independentemente de conjunto também é usado. CONJUNTO de fornece um comutador virtual com alguns dos mesmos recursos que o agrupamento NIC. Para obter detalhes, consulte [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).  
  
-   **Máquina virtual com várias filas (VMMQ)**. Melhora a taxa de transferência VMQ alocando várias filas de hardware por máquina virtual.  A fila padrão torna-se um conjunto de filas para uma máquina virtual e o tráfego é distribuído entre as filas.  
  
-   **Qualidade de serviço (QoS) para redes definidas por software**. Gerencia a classe padrão de tráfego através do comutador virtual dentro da largura de banda de classe padrão.  
  
Para obter mais informações sobre novos recursos de rede, consulte [o que há de novo no sistema de rede](../../networking/What-s-New-in-Networking.md).  
  
## <a name="BKMK_check"></a>Pontos de verificação de produção \(novo\)
Pontos de verificação de produção são imagens de "point-in-time" de uma máquina virtual. Eles oferecem uma maneira de aplicar um ponto de verificação está em conformidade com as políticas de suporte quando a máquina virtual executa uma carga de trabalho de produção. Pontos de verificação de produção são baseados na tecnologia de backup no convidado em vez de um estado salvo. Para as máquinas virtuais do Windows, o serviço de instantâneo de Volume (VSS) é usado. Para máquinas virtuais Linux, os buffers do sistema de arquivos são liberados para criar um ponto de verificação consistente com o sistema de arquivos. Se você preferir usar pontos de verificação com base em estados salvos, em vez disso, a escolha de pontos de verificação padrão. Para obter detalhes, consulte [escolha entre pontos de verificação padrão ou de produção no Hyper-V](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).  
  
> [!IMPORTANT]  
> Novas máquinas virtuais usam pontos de verificação de produção como o padrão.  
  
## <a name="BKMK_HyperVRollingUpgrades"></a>Atualização do Cluster Hyper-V sem interrupção \(novo\) 
Agora você pode adicionar um nó que executa o Windows Server 2016 em um Cluster do Hyper-V conosco que executam o Windows Server 2012 R2. Isso permite que você atualize o cluster sem tempo de inatividade. O cluster é executado em um nível de recurso do Windows Server 2012 R2, até que você atualize todos os nós no cluster e atualize o nível funcional do cluster com o cmdlet do Windows PowerShell [Update-ClusterFunctionalLevel](https://docs.microsoft.com/powershell/module/failoverclusters/Update-ClusterFunctionalLevel).  
  
> [!IMPORTANT]  
> Depois de atualizar o nível funcional do cluster, você não pode retorná-lo para o Windows Server 2012 R2.  
  
Para um cluster do Hyper-V com um nível funcional do Windows Server 2012 R2 conosco que executam o Windows Server 2012 R2 e Windows Server 2016, observe o seguinte:  
  
-   Gerencie o cluster, Hyper-V e máquinas virtuais de um nó que executa o Windows Server 2016 ou Windows 10.  
  
-   Você pode mover as máquinas virtuais entre todos os nós no cluster do Hyper-V.  
  
-   Para usar os novos recursos do Hyper-V, todos os nós devem executar o Windows Server 2016 e o nível funcional do cluster deve ser atualizado.  
  
-   A versão de configuração de máquina virtual para máquinas virtuais existentes não é atualizada. Você pode atualizar a versão de configuração somente depois de atualizar o nível funcional do cluster.  
  
-   Máquinas virtuais que você cria são compatíveis com o Windows Server 2012 R2, o nível de configuração de máquina virtual 5.  
  
Depois que você atualize o nível funcional do cluster:  
  
-   Você pode habilitar novos recursos do Hyper-V.  
  
-   Para disponibilizar novos recursos de máquina virtual, use o cmdlet Update-VmConfigurationVersion para atualizar manualmente o nível de configuração de máquina virtual. Para obter instruções, consulte [máquinas virtuais de atualização de versão](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).    
-   Você não pode adicionar um nó ao Cluster do Hyper-V que executa o Windows Server 2012 R2.  
  
> [!NOTE]  
> Hyper-V no Windows 10 não oferece suporte a clustering de failover.  
  
Para obter detalhes e instruções, consulte a [atualização sem interrupção do Cluster de sistema operacional](https://technet.microsoft.com/library/dn850430.aspx).  

## <a name="BKMK_shared"></a>Discos rígidos virtuais compartilhados \(atualizado\)
Agora você pode redimensionar os discos rígidos virtuais compartilhados (arquivos. vhdx) usados para o cluster convidado, sem tempo de inatividade. Discos rígidos virtuais compartilhados pode ser aumentados ou reduzidos enquanto a máquina virtual está online. Clusters de convidado também agora podem proteger discos rígidos virtuais compartilhados usando a réplica do Hyper-V para recuperação de desastres.

Habilite a replicação na coleção. Habilitar a replicação em uma coleção é **só são expostas por meio da interface WMI**. Consulte a documentação para [Msvm_CollectionReplicationService classe](https://msdn.microsoft.com/library/mt167787%28v=vs.85%29.aspx) para obter mais detalhes. **Você não pode gerenciar a replicação de uma coleção por meio do cmdlet do PowerShell ou da interface do usuário.** As VMs devem estar em hosts que fazem parte de um cluster do Hyper-V para acessar os recursos que são específicos a uma coleção. Isso inclui o VHD compartilhado - compartilhadas VHDs em hosts autônomos não são compatíveis com a réplica do Hyper-V.

Siga as diretrizes para VHDs compartilhados no [Virtual Hard Disk Sharing Overview](https://technet.microsoft.com/library/dn281956.aspx)e certifique-se de que seus VHDs compartilhadas fazem parte de um cluster de convidado. 

Uma coleção com um VHD compartilhado, mas nenhum cluster de convidado associado não é possível criar pontos de referência para a coleção (independentemente se o VHD compartilhado é incluído na criação de ponto de referência ou não). 

## <a name="virtual-machine-backupnew"></a>Backup de máquinas virtuais\(novo\)

Se você estiver fazendo backup de uma única máquina virtual (independentemente se o host está clusterizado ou não), você não deve usar um grupo VM.  Nem você deve usar uma coleção de instantâneo. Grupos VM e coleta de instantâneo destinam-se a ser usada exclusivamente para fazer backup de clusters de convidados que estão usando o vhdx compartilhado. Em vez disso, você deve tirar um instantâneo usando o [provedor do WMI do Hyper-V v2](https://msdn.microsoft.com/library/windows/desktop/hh850319(v=vs.85).aspx). Da mesma forma, não use o [provedor WMI do Cluster de Failover](https://msdn.microsoft.com/library/windows/desktop/mt167750(v=vs.85).aspx).

## <a name="BKMK_shielded"></a>Máquinas virtuais blindadas \(novo\)
Blindado uso de máquinas virtuais com vários recursos para tornar mais difícil para os administradores do Hyper-V e o malware no host inspecionar, violar ou roubar dados do estado de uma máquina virtual blindada. Dados e o estado é criptografado, os administradores do Hyper-V não conseguir ver a saída de vídeo e os discos e as máquinas virtuais podem ser restritas a serem executados somente em hosts conhecidos, íntegros, conforme determinado por um servidor de guardião de Host. Para obter detalhes, consulte [malha protegida e VMs Blindadas](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md).
  
>[!NOTE]  
> Máquinas virtuais blindadas são compatíveis com a réplica do Hyper-V. Para replicar uma máquina virtual blindada, o host que você deseja replicar para deve ser autorizado a executar essa máquina virtual blindada.  

## <a name="BKMK_StartOrder"></a>Iniciar a prioridade de ordem para máquinas virtuais em cluster \(novo\)
Esse recurso oferece a você mais controle sobre o qual as máquinas virtuais em cluster são iniciadas ou reiniciadas pela primeira vez. Isso torna mais fácil de iniciar as máquinas virtuais que fornecem serviços antes de máquinas virtuais que usam esses serviços. Definir conjuntos, colocar as máquinas virtuais em conjuntos e especificar dependências. Usar cmdlets do Windows PowerShell para gerenciar os conjuntos, tais como [New-ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/new-clustergroupset), [Get-ClusterGroupSet](https://docs.microsoft.com/powershell/module/failoverclusters/get-clustergroupset), e [ClusterGroupSetDependency adicionar](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergroupsetdependency).
.  
## <a name="BKMK_QoS"></a>Qualidade de serviço (QoS) de armazenamento \(atualizado\)
Agora você pode criar políticas de QoS de armazenamento em um Servidor de Arquivos de Escalabilidade Horizontal e atribuí-las a um ou mais discos virtuais em máquinas virtuais Hyper-V. O desempenho de armazenamento será reajustado automaticamente para atender às políticas à medida que a carga de armazenamento flutua. Para obter detalhes, consulte [qualidade de serviço do armazenamento](../../storage/storage-qos/storage-qos-overview.md).  
  
## <a name="BKMK_Config"></a>Formato de arquivo de configuração de máquina virtual \(atualizado\)
Arquivos de configuração de máquina virtual usam um novo formato que torna a leitura e gravação de dados de configuração mais eficientes. O formato também torna a corrupção de dados menor probabilidade de se ocorrer uma falha de armazenamento. Arquivos de dados de configuração de máquina virtual usam uma extensão de nome de arquivo. vmcx e arquivos de dados de estado de tempo de execução usar uma extensão de nome de arquivo. vmrs.  
  
> [!IMPORTANT]  
> A extensão de nome de arquivo. vmcx indica um arquivo binário. Não há suporte a edição de arquivos. vmcx ou. vmrs.  
  
## <a name="BKMK_ConfgVersion"></a>Versão de configuração de máquina virtual \(atualizado\)
A versão representa a compatibilidade da configuração da máquina virtual, salva o estado e os arquivos de instantâneo com a versão do Hyper-V. Máquinas virtuais com a versão 5 são compatíveis com o Windows Server 2012 R2 e pode executar no Windows Server 2012 R2 e Windows Server 2016. Máquinas virtuais com as versões introduzidas no Windows Server 2016 não será executado no Hyper-V no Windows Server 2012 R2.   
  
Se você mover ou importa uma máquina virtual a um servidor que executa o Hyper-V no Windows Server 2016 do Windows Server 2012 R2, a configuração de máquina virtual não será atualizada automaticamente. Isso significa que você pode mover a máquina virtual de volta para um servidor que executa o Windows Server 2012 R2. Mas, isso também significa que você não pode usar os novos recursos de máquina virtual até atualizar manualmente a versão da configuração da máquina virtual.  
  
Para obter instruções sobre como verificar e atualizar a versão, consulte [máquinas virtuais de atualização de versão](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Este artigo também lista a versão em que alguns recursos foram introduzidos.   
  
> [!IMPORTANT]  
> -   Depois de atualizar a versão, é possível mover a máquina virtual a um servidor que executa o Windows Server 2012 R2.  
> -   Você não pode reverter a configuração para uma versão anterior.  
> -   O [VMVersion atualização](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion) cmdlet é bloqueado em um Cluster do Hyper-V quando o nível funcional do cluster é o Windows Server 2012 R2.  

## <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>Segurança Virtualization-based para máquinas virtuais de geração 2 \(novo)
Segurança baseada em virtualização habilita recursos como o Device Guard e Credential Guard, oferecendo maior proteção do sistema operacional contra explorações de malware. Segurança baseada em virtualização está disponível em máquinas virtuais da geração 2 convidados começando com a versão 8. Para obter informações sobre a versão da máquina virtual, consulte [máquinas virtuais de atualização de versão no Hyper-V no Windows 10 ou Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).


## <a name="BKMK_Containers"></a>Contêineres do Windows \(novo\) 
Contêineres do Windows permitem que vários aplicativos isolados executar em um sistema de computador. Eles são rápidos criar e são altamente escalonáveis e portátil. Dois tipos de tempo de execução do contêiner estão disponíveis, cada um com um grau de isolamento de aplicativo diferente. Contêineres do Windows Server usar o isolamento de processo e de namespace. Contêineres de Hyper-V usam uma máquina virtual de leve para cada contêiner.  
  
Os principais recursos incluem:  
  
-   Suporte para sites e aplicativos usando HTTPS  
  
-   Nano server pode hospedar o Windows Server e contêineres do Hyper-V  
  
-   Capacidade de gerenciar dados em pastas de contêiner compartilhadas  
  
-   Capacidade de restringir os recursos do contêiner  
  
Para obter detalhes, incluindo guias de início rápido, consulte o [documentação de contêineres do Windows](https://docs.microsoft.com/virtualization/windowscontainers/index).  
  
## <a name="BKMK_PowerShellDirect"></a>Windows PowerShell Direct \(novo\)  
Isso lhe dá uma maneira para executar comandos do Windows PowerShell em uma máquina virtual do host. Windows PowerShell Direct é executado entre o host e a máquina virtual. Isso significa que ele não requer requisitos de firewall ou de rede, e ele funciona independentemente de sua configuração de gerenciamento remoto.  
  
Direto do Windows PowerShell é uma alternativa para as ferramentas existentes que os administradores do Hyper-V usam para se conectar a uma máquina virtual em um host Hyper-V:  
  
-   Ferramentas de gerenciamento remoto como o PowerShell ou a Área de Trabalho Remota  
  
-   Conexão de máquina Virtual Hyper-V (VMConnect)  
  
Essas ferramentas funcionam bem, mas têm vantagens e desvantagens: VMConnect é confiável, mas pode ser difícil de automatizar. PowerShell remoto é eficiente, mas pode ser difícil de configurar e manter. Essas compensações podem se tornar mais importantes à medida que aumenta sua implantação do Hyper-V. Direto do Windows PowerShell lida com isso ao oferecer uma experiência de scripts e automação poderosa que é tão simple quanto usar o VMConnect.   
  
Para requisitos e instruções, consulte [máquinas virtuais de gerenciar o Windows com o PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md).  
