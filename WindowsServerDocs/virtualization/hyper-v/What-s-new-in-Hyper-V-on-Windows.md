---
title: O que há de novo no Hyper-V no Windows Server 2016
description: Fornece um resumo dos novos recursos no Hyper-V
ms.topic: article
ms.assetid: 1a65a98e-54b6-4c41-9732-1e3d32fe3a5f
ms.author: benarm
author: BenjaminArmstrong
ms.date: 09/21/2017
ms.openlocfilehash: 2a6107468e63f819e1957db0736c1a07c5dc24a0
ms.sourcegitcommit: faa5db4cdba4ad2b3a65533b6b49d960080923c9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/06/2020
ms.locfileid: "91752880"
---
# <a name="whats-new-in-hyper-v-on-windows-server"></a>O que há de novo no Hyper-V no Windows Server

>Aplica-se a: Windows Server 2019, Microsoft Hyper-V Server 2016, Windows Server 2016

Este artigo explica as funcionalidades novas e alteradas do Hyper-V no Windows Server 2019, no Windows Server 2016 e no Microsoft Hyper-V Server 2016. Para usar novos recursos em máquinas virtuais criadas com o Windows Server 2012 R2 e movido ou importado para um servidor que executa o Hyper-V no Windows Server 2019 ou no Windows Server 2016, você precisará atualizar manualmente a versão de configuração da máquina virtual. Para obter instruções, consulte [atualizar a versão da máquina virtual](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).

Aqui estão as novidades incluídas neste artigo e se a funcionalidade é nova ou atualizada.

## <a name="windows-server-version-1903"></a>Windows Server, versão 1903

### <a name="add-hyper-v-manager-to-server-core-installations-updated"></a>Adicionar o Gerenciador do Hyper-V às instalações do Server Core (atualizado)

Como você deve saber, é recomendável usar a opção de instalação Server Core ao usar o Windows Server, Canal Semestral, na produção. No entanto, o Server Core por padrão omite uma série de ferramentas de gerenciamento úteis. Você pode adicionar muitas das ferramentas normalmente mais usadas instalando o recurso de compatibilidade de aplicativos, mas há algumas ferramentas ausentes.

Portanto, com base nos comentários dos clientes, adicionamos mais uma ferramenta ao recurso de compatibilidade de aplicativos nesta versão: Gerenciador do Hyper-V (virtmgmt. msc).

Para obter mais informações, confira [Recurso de compatibilidade de aplicativos do Server Core](../../get-started-19/install-fod-19.md).

## <a name="windows-server-2019"></a>Windows Server 2019

### <a name="security-shielded-virtual-machines-improvements-new"></a>Segurança: aprimoramentos de máquinas virtuais blindadas (novas)

- **Melhorias nas filiais**

    Agora você pode executar máquinas virtuais blindadas em computadores com conectividade intermitente ao Serviço Guardião de Host, aproveitando os novos recursos de [HGS de fallback](../../security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office.md#fallback-configuration) e [modo offline](../../security/guarded-fabric-shielded-vm/guarded-fabric-manage-branch-office.md#offline-mode) . O HGS de fallback permite que você configure um segundo conjunto de URLs para o Hyper-V tentar caso não consiga acessar seu servidor HGS principal.

    O modo offline permite que você continue a iniciar suas VMs blindadas, mesmo se o HGS não puder ser alcançado, contato que a VM tenha sido iniciada com êxito pelo menos uma vez e as configurações de segurança do host não tenham sido alteradas desde então.

- **Melhorias na solução de problemas**

    Também facilitamos o processo para [solucionar problemas de suas máquinas virtuais blindadas](../../security/guarded-fabric-shielded-vm/guarded-fabric-troubleshoot-shielded-vms.md) habilitando o suporte para o modo de sessão avançado VMConnect e o PowerShell Direct. Essas ferramentas são particularmente úteis se você tiver perdido a conectividade de rede com sua VM e precisar atualizar a configuração dela para restaurar o acesso.

    Esses recursos não precisam ser configurados e são disponibilizados automaticamente quando uma VM blindada é posta em um host Hyper-V que executa o Windows Server versão 1803 ou mais recente.

- **Suporte a Linux**

    Se você executa ambientes com múltiplos sistemas operacionais, agora o Windows Server 2019 oferece suporte para executar o Ubuntu, o Red Hat Enterprise Linux e o SUSE Linux Enterprise Server em máquinas virtuais blindadas.

## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="compatible-with-connected-standby-new"></a>Compatível com a nova em espera conectada \(\)

Quando a função Hyper-V é instalada em um computador que usa o modelo de energia Sempre ativo/conectado (AOAC), o estado de energia em **espera conectado** agora está disponível.

### <a name="discrete-device-assignment-new"></a>Nova atribuição de dispositivo discreta \(\)

Esse recurso permite dar a uma máquina virtual acesso direto e exclusivo a alguns dispositivos de hardware PCIe. Usar um dispositivo dessa maneira ignora a pilha de virtualização do Hyper-V, o que resulta em um acesso mais rápido. Para obter detalhes sobre o hardware com suporte, consulte "atribuição de dispositivo discreta" em [requisitos de sistema para o Hyper-V no Windows Server 2016](System-requirements-for-Hyper-V-on-Windows.md). Para obter detalhes, incluindo como usar esse recurso e considerações, consulte a postagem "[atribuição de dispositivo discreta — descrição e plano de fundo](https://blogs.technet.microsoft.com/virtualization/2015/11/19/discrete-device-assignment-description-and-background/)" no blog de virtualização.

### <a name="encryption-support-for-the-operating-system-disk-in-generation-1-virtual-machines-new"></a>Suporte de criptografia para o disco do sistema operacional em máquinas virtuais de geração 1 \( novo)

Agora você pode proteger o disco do sistema operacional usando a criptografia de unidade de disco BitLocker em máquinas virtuais de geração 1. Um novo recurso, armazenamento de chaves, cria uma unidade pequena e dedicada para armazenar a chave do BitLocker da unidade do sistema. Isso é feito em vez de usar um Trusted Platform Module virtual (TPM), que está disponível somente em máquinas virtuais de geração 2. Para descriptografar o disco e iniciar a máquina virtual, o host Hyper-V deve fazer parte de uma malha protegida autorizada ou ter a chave privada de um dos Guardiões da máquina virtual. O armazenamento de chaves requer uma máquina virtual de versão 8. Para obter informações sobre a versão da máquina virtual, consulte [atualizar a versão da máquina virtual no Hyper-V no Windows 10 ou no Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).

### <a name="host-resource-protection-new"></a>Nova proteção de recurso de host \(\)

Esse recurso ajuda a impedir que uma máquina virtual use mais do que sua participação de recursos do sistema, procurando níveis excessivos de atividade. Isso pode ajudar a impedir que a atividade excessiva de uma máquina virtual prejudique o desempenho do host ou de outras máquinas virtuais. Quando o monitoramento detecta uma máquina virtual com atividade excessiva, a máquina virtual recebe menos recursos. Esse monitoramento e imposição estão desativados por padrão. Use o Windows PowerShell para ativar ou desativar. Para ativá-lo, execute este comando:

```
Set-VMProcessor TestVM -EnableHostResourceProtection $true
```

Para obter detalhes sobre esse cmdlet, consulte [set-VMProcessor](/powershell/module/hyper-v/set-vmprocessor).

### <a name="hot-add-and-remove-for-network-adapters-and-memory-new"></a>Adição e remoção ativas para adaptadores de rede e \( nova memória\)

Agora você pode adicionar ou remover um adaptador de rede enquanto a máquina virtual está em execução, sem incorrer em tempo de inatividade. Isso funciona para máquinas virtuais de geração 2 que executam sistemas operacionais Windows ou Linux.

Você também pode ajustar a quantidade de memória atribuída a uma máquina virtual enquanto ela está em execução, mesmo que você não tenha habilitado Memória Dinâmica. Isso funciona para máquinas virtuais de geração 1 e de geração 2, executando o Windows Server 2016 ou o Windows 10.

### <a name="hyper-v-manager-improvements-updated"></a>Melhorias do Gerenciador do Hyper-V \( atualizadas\)

-   **Suporte a credenciais alternativas** -agora você pode usar um conjunto diferente de credenciais no Gerenciador do Hyper-V ao conectar-se a outro host remoto do windows Server 2016 ou do Windows 10. Você também pode salvar essas credenciais para facilitar o logon novamente.

-   **Gerenciar versões anteriores** – com o Gerenciador do Hyper-V no windows Server 2019, windows Server 2016 e Windows 10, você pode gerenciar computadores que executam o Hyper-v no windows Server 2012, Windows 8, windows Server 2012 R2 e Windows 8.1.

-   **Protocolo de gerenciamento atualizado** – o Gerenciador do Hyper-v agora se comunica com hosts remotos do Hyper-v usando o protocolo WS-Man, que permite a autenticação CredSSP, Kerberos ou NTLM. Ao usar o CredSSP para se conectar a um host remoto do Hyper-V, você pode fazer uma migração ao vivo sem habilitar a delegação restrita no Active Directory. A infraestrutura baseada no WS-MAN também facilita a habilitação de um host para o gerenciamento remoto. O WS-MAN se conecta pela porta 80, que é aberta por padrão.

### <a name="integration-services-delivered-through-windows-update-updated"></a>Serviços de integração fornecidos por Windows Update \( atualizados\)

As atualizações do Integration Services para convidados do Windows são distribuídas por meio de Windows Update. Para provedores de serviços e hosts de nuvem privada, isso coloca o controle da aplicação de atualizações nas mãos dos locatários que possuem as máquinas virtuais. Os locatários agora podem atualizar suas máquinas virtuais do Windows com todas as atualizações, incluindo o Integration Services, usando um único método. Para obter detalhes sobre o Integration Services para convidados do Linux, consulte [máquinas virtuais Linux e FreeBSD no Hyper-V](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md).

> [!IMPORTANT]
> O arquivo de imagem vmguest. ISO não é mais necessário, portanto não está incluído no Hyper-V no Windows Server 2016.

### <a name="linux-secure-boot-new"></a>Nova inicialização segura do Linux \(\)

Os sistemas operacionais Linux em execução em máquinas virtuais de geração 2 agora podem ser inicializados com a opção de inicialização segura habilitada. Ubuntu 14, 4 e posterior, SUSE Linux Enterprise Server 12 e posterior, Red Hat Enterprise Linux 7,0 e posterior e CentOS 7,0 e posterior são habilitados para inicialização segura em hosts que executam o Windows Server 2016. Antes de inicializar a máquina virtual pela primeira vez, você deve configurar a máquina virtual para usar a autoridade de certificação UEFI da Microsoft. Você pode fazer isso no Gerenciador do Hyper-V, Virtual Machine Manager ou em uma sessão elevada do Windows PowerShell. Para o Windows PowerShell, execute este comando:

```
Set-VMFirmware TestVM -SecureBootTemplate MicrosoftUEFICertificateAuthority
```

Para obter mais informações sobre máquinas virtuais do Linux no Hyper-V, consulte [máquinas virtuais Linux e FreeBSD no Hyper-v](Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md). Para obter mais informações sobre o cmdlet, consulte [set-VMFirmware](/powershell/module/hyper-v/set-vmfirmware).

### <a name="more-memory-and-processors-for-generation-2-virtual-machines-and-hyper-v-hosts-updated"></a>Mais memória e processadores para máquinas virtuais de geração 2 e hosts do Hyper-V \( atualizados\)

A partir da versão 8, as máquinas virtuais de geração 2 podem usar significativamente mais memória e processadores virtuais. Os hosts também podem ser configurados com significativamente mais memória e processadores virtuais do que o suportado anteriormente. Essas alterações dão suporte a novos cenários, como a execução de grandes bancos de dados na memória de comércio eletrônico para OLTP (processamento de transações online) e data warehousing (DW). O blog do Windows Server publicou recentemente os resultados de desempenho de uma máquina virtual com 5,5 terabytes de memória e 128 processadores virtuais que executam 4 TB de banco de dados na memória. O desempenho foi maior que 95% do desempenho de um servidor físico. Para obter detalhes, consulte [desempenho da VM em grande escala do Windows Server 2016 Hyper-V para processamento de transações na memória](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Para obter detalhes sobre as versões da máquina virtual, consulte [atualizar a versão da máquina virtual no Hyper-V no Windows 10 ou no Windows Server 2016](./deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Para obter a lista completa de configurações máximas com suporte, consulte [planejar a escalabilidade do Hyper-V no Windows Server 2016](./plan/plan-hyper-v-scalability-in-windows-server.md).

### <a name="nested-virtualization-new"></a>Nova virtualização aninhada \(\)

Esse recurso permite que você use uma máquina virtual como um host Hyper-V e crie máquinas virtuais dentro desse host virtualizado. Isso pode ser especialmente útil para ambientes de desenvolvimento e teste. Para usar a virtualização aninhada, você precisará de:

-   Para executar pelo menos o Windows Server 2019, o Windows Server 2016 ou o Windows 10 no host do Hyper-V físico e no host virtualizado.

-   Um processador com Intel VT-x (a virtualização aninhada está disponível apenas para processadores Intel neste momento).

Para obter detalhes e instruções, consulte [executar o Hyper-V em uma máquina virtual com virtualização aninhada](/virtualization/hyper-v-on-windows/user-guide/nested-virtualization).

### <a name="networking-features-new"></a>Novos recursos de rede \(\)

Os novos recursos de rede incluem:

-   **Acesso remoto direto à memória (RDMA) e Comutador incorporado (conjunto)**. Você pode configurar o RDMA em adaptadores de rede vinculados a um comutador virtual do Hyper-V, independentemente de o conjunto também ser usado. SET fornece um comutador virtual com alguns dos mesmos recursos que o agrupamento NIC. Para obter detalhes, consulte [acesso remoto direto à memória (RDMA) e comutador inserido de equipe (Set)](../hyper-v-virtual-switch/rdma-and-switch-embedded-teaming.md).

-   **VMMQ (várias filas de máquina virtual)**. Melhora a taxa de transferência de VMQ alocando várias filas de hardware por máquina virtual.  A fila padrão torna-se um conjunto de filas para uma máquina virtual e o tráfego é distribuído entre as filas.

-   **QoS (qualidade de serviço) para redes definidas por software**. Gerencia a classe padrão de tráfego por meio do comutador virtual dentro da largura de banda de classe padrão.

Para obter mais informações sobre novos recursos de rede, consulte [novidades na rede](../../networking/What-s-New-in-Networking.md).

### <a name="production-checkpoints-new"></a>Novos pontos de verificação de produção \(\)

Os pontos de verificação de produção são imagens "point-in-time" de uma máquina virtual. Eles fornecem uma maneira de aplicar um ponto de verificação que esteja em conformidade com as políticas de suporte quando uma máquina virtual executar uma carga de trabalho de produção. Os pontos de verificação de produção são baseados na tecnologia de backup dentro do convidado, em vez de um estado salvo. Para máquinas virtuais do Windows, o VSS (serviço de instantâneo de volume) é usado. Para máquinas virtuais do Linux, os buffers do sistema de arquivos são liberados para criar um ponto de verificação consistente com o sistema de arquivos. Se você preferir usar pontos de verificação com base em Estados salvos, escolha pontos de verificação padrão em vez disso. Para obter detalhes, consulte [escolher entre pontos de verificação padrão ou de produção no Hyper-V](manage/Choose-between-standard-or-production-checkpoints-in-Hyper-V.md).

> [!IMPORTANT]
> As novas máquinas virtuais usam pontos de verificação de produção como padrão.

### <a name="rolling-hyper-v-cluster-upgrade-new"></a>Nova atualização de cluster do Hyper-V \(\)

Agora você pode adicionar um nó que executa o Windows Server 2019 ou o Windows Server 2016 a um cluster Hyper-V com nós que executam o Windows Server 2012 R2. Isso permite que você atualize o cluster sem tempo de inatividade. O cluster é executado em um nível de recurso do Windows Server 2012 R2 até que você atualize todos os nós no cluster e atualize o nível funcional do cluster com o cmdlet do Windows PowerShell, [Update-ClusterFunctionalLevel](/powershell/module/failoverclusters/Update-ClusterFunctionalLevel).

> [!IMPORTANT]
> Depois de atualizar o nível funcional do cluster, você não poderá retorná-lo ao Windows Server 2012 R2.

Para um cluster Hyper-V com um nível funcional do Windows Server 2012 R2 com nós que executam o Windows Server 2012 R2, o Windows Server 2019 e o Windows Server 2016, observe o seguinte:

-   Gerencie o cluster, o Hyper-V e as máquinas virtuais de um nó que executa o Windows Server 2016 ou o Windows 10.

-   Você pode mover máquinas virtuais entre todos os nós no cluster do Hyper-V.

-   Para usar os novos recursos do Hyper-V, todos os nós devem executar o Windows Server 2016 ou o nível funcional do cluster deve ser atualizado.

-   A versão de configuração de máquina virtual para máquinas virtuais existentes não está atualizada. Você pode atualizar a versão de configuração somente depois de atualizar o nível funcional do cluster.

-   As máquinas virtuais que você cria são compatíveis com o Windows Server 2012 R2, nível de configuração de máquina virtual 5.

Depois de atualizar o nível funcional do cluster:

-   Você pode habilitar novos recursos do Hyper-V.

-   Para disponibilizar novos recursos de máquina virtual, use o `Update-vmVersion` cmdlet para atualizar manualmente o nível de configuração da máquina virtual. Para obter instruções, consulte [atualizar a versão da máquina virtual](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md).
-   Você não pode adicionar um nó ao cluster Hyper-V que executa o Windows Server 2012 R2.

> [!NOTE]
> O Hyper-V no Windows 10 não dá suporte ao clustering de failover.

Para obter detalhes e instruções, consulte a [atualização sem interrupção do sistema operacional do cluster](https://technet.microsoft.com/library/dn850430.aspx).

### <a name="shared-virtual-hard-disks-updated"></a>Discos rígidos virtuais compartilhados \( atualizados\)
Agora você pode redimensionar os discos rígidos virtuais compartilhados (arquivos. vhdx) usados para clustering de convidado, sem tempo de inatividade. Os discos rígidos virtuais compartilhados podem ser expandidos ou reduzidos enquanto a máquina virtual está online. Os clusters convidados agora também podem proteger discos rígidos virtuais compartilhados usando a réplica do Hyper-V para recuperação de desastre.

Habilite a replicação na coleção. Habilitar a replicação em uma coleção **só é exposto por meio da interface WMI**. Consulte a documentação para [Msvm_CollectionReplicationService classe](/previous-versions/windows/desktop/clushyperv/msvm-collectionreplicationservice) para obter mais detalhes. **Você não pode gerenciar a replicação de uma coleção por meio do cmdlet do PowerShell ou da interface do usuário.** As VMs devem estar em hosts que fazem parte de um cluster Hyper-V para acessar recursos que são específicos de uma coleção. Isso inclui VHDs compartilhados de VHD compartilhados em hosts autônomos não são suportados pela réplica do Hyper-V.

Siga as diretrizes para VHDs compartilhados em [visão geral do compartilhamento de disco rígido virtual](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn281956(v=ws.11))e certifique-se de que seus VHDs compartilhados façam parte de um cluster de convidado.

Uma coleção com um VHD compartilhado, mas nenhum cluster convidado associado não pode criar pontos de referência para a coleção (independentemente de o VHD compartilhado estar incluído na criação do ponto de referência ou não).

### <a name="virtual-machine-backupnew"></a>Novo backup de máquina virtual \(\)

Se você estiver fazendo backup de uma única máquina virtual (independentemente de o host estar clusterizado ou não), não deverá usar um grupo de VMs.  Nem você deve usar uma coleção de instantâneos. Os grupos de VMs e a coleção de instantâneos devem ser usados exclusivamente para fazer backup de clusters convidados que estejam usando vhdx compartilhado. Em vez disso, você deve fazer um instantâneo usando o [provedor de WMI v2 do Hyper-V](/windows/win32/hyperv_v2/windows-virtualization-portal). Da mesma forma, não use o [provedor WMI do cluster de failover](/previous-versions/windows/desktop/clushyperv/failover-clustering-hyper-v-wmi-provider-portal).

### <a name="shielded-virtual-machines-new"></a>Novas máquinas virtuais blindadas \(\)

As máquinas virtuais blindadas usam vários recursos para tornar mais difícil para os administradores do Hyper-V e o malware no host inspecionar, adulterar ou roubar dados do estado de uma máquina virtual blindada. Dados e estado são criptografados, os administradores do Hyper-V não podem ver a saída e os discos de vídeo, e as máquinas virtuais podem ser restritas para serem executadas somente em hosts íntegros conhecidos, conforme determinado por um servidor guardião de host. Para obter detalhes, consulte [malha protegida e VMs blindadas](../../security/guarded-fabric-shielded-vm/guarded-fabric-and-shielded-vms.md).

> [!NOTE]
> As máquinas virtuais blindadas são compatíveis com a réplica do Hyper-V. Para replicar uma máquina virtual blindada, o host no qual você deseja replicar deve ser autorizado a executar essa máquina virtual blindada.

### <a name="start-order-priority-for-clustered-virtual-machines-new"></a>Prioridade da ordem de início para máquinas virtuais clusterizadas \( novo\)

Esse recurso oferece mais controle sobre quais máquinas virtuais clusterizadas são iniciadas ou reiniciadas primeiro. Isso torna mais fácil iniciar as máquinas virtuais que fornecem serviços antes das máquinas virtuais que usam esses serviços. Defina conjuntos, coloque máquinas virtuais em conjuntos e especifique dependências. Use os cmdlets do Windows PowerShell para gerenciar os conjuntos, como [New-ClusterGroupSet](/powershell/module/failoverclusters/new-clustergroupset), [Get-ClusterGroupSet](/powershell/module/failoverclusters/get-clustergroupset)e [Add-ClusterGroupSetDependency](/powershell/module/failoverclusters/add-clustergroupsetdependency).
.
### <a name="storage-quality-of-service-qos-updated"></a>Qualidade de serviço (QoS) de armazenamento \( atualizada\)

Agora você pode criar políticas de QoS de armazenamento em um Servidor de Arquivos de Escalabilidade Horizontal e atribuí-las a um ou mais discos virtuais em máquinas virtuais Hyper-V. O desempenho de armazenamento será reajustado automaticamente para atender às políticas à medida que a carga de armazenamento flutua. Para obter detalhes, consulte [qualidade de serviço de armazenamento](../../storage/storage-qos/storage-qos-overview.md).

### <a name="virtual-machine-configuration-file-format-updated"></a>Formato de arquivo de configuração de máquina virtual \( atualizado\)

Os arquivos de configuração de máquina virtual usam um novo formato que torna mais eficiente a leitura e gravação de dados de configuração. O formato também torna a corrupção de dados menos provável se ocorrer uma falha de armazenamento. Os arquivos de dados de configuração de máquina virtual usam uma extensão de nome de arquivo. vmcx e arquivos de dados de estado de tempo de execução usam uma extensão de nome de arquivo. VMRS

> [!IMPORTANT]
> A extensão de nome de arquivo. vmcx indica um arquivo binário. Não há suporte para a edição de arquivos. vmcx ou. VMRS.

### <a name="virtual-machine-configuration-version-updated"></a>Versão de configuração de máquina virtual \( atualizada\)

A versão representa a compatibilidade da configuração da máquina virtual, do estado salvo e dos arquivos de instantâneo com a versão do Hyper-V. As máquinas virtuais com a versão 5 são compatíveis com o Windows Server 2012 R2 e podem ser executadas no Windows Server 2012 R2 e no Windows Server 2016. As máquinas virtuais com versões introduzidas no Windows Server 2016 e no Windows Server 2019 não serão executadas no Hyper-V no Windows Server 2012 R2.

Se você mover ou importar uma máquina virtual para um servidor que executa o Hyper-V no Windows Server 2016 ou no Windows Server 2019 do Windows Server 2012 R2, a configuração da máquina virtual não será atualizada automaticamente. Isso significa que você pode mover a máquina virtual de volta para um servidor que executa o Windows Server 2012 R2. Mas isso também significa que você não pode usar os novos recursos de máquina virtual até atualizar manualmente a versão da configuração da máquina virtual.

Para obter instruções sobre como verificar e atualizar a versão, consulte [atualizar a versão da máquina virtual](deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Este artigo também lista a versão na qual alguns recursos foram introduzidos.

> [!IMPORTANT]
> -   Depois de atualizar a versão, você não pode mover a máquina virtual para um servidor que executa o Windows Server 2012 R2.
> -   Não é possível fazer downgrade da configuração para uma versão anterior.
> -   O cmdlet [Update-VMVersion](/powershell/module/hyper-v/update-vmversion) é bloqueado em um cluster do Hyper-V quando o nível funcional do cluster é o Windows Server 2012 R2.

### <a name="virtualization-based-security-for-generation-2-virtual-machines-new"></a>Segurança baseada em virtualização para máquinas virtuais de geração 2 \( novas)

A segurança baseada em virtualização capacita recursos como o Device Guard e o Credential Guard, oferecendo maior proteção do sistema operacional contra explorações de malware. Com base na virtualização, a segurança está disponível em máquinas virtuais de convidado da geração 2 a partir da versão 8. Para obter informações sobre a versão da máquina virtual, consulte [atualizar a versão da máquina virtual no Hyper-V no Windows 10 ou no Windows Server 2016](./deploy/upgrade-virtual-machine-version-in-hyper-v-on-windows-or-windows-server.md).

### <a name="windows-containers-new"></a>Novos contêineres do Windows \(\)

Os contêineres do Windows permitem que muitos aplicativos isolados sejam executados em um sistema de computador. Eles são rápidos de criar e são altamente escalonáveis e portáteis. Dois tipos de tempo de execução de contêiner estão disponíveis, cada um com um grau diferente de isolamento de aplicativo. Os contêineres do Windows Server usam o isolamento de namespace e de processo. Os contêineres do Hyper-V usam uma máquina virtual leve para cada contêiner.

Os principais recursos incluem:

-   Suporte para sites e aplicativos usando HTTPS

-   O nano Server pode hospedar os contêineres do Windows Server e do Hyper-V

-   Capacidade de gerenciar dados por meio de pastas compartilhadas de contêiner

-   Capacidade de restringir os recursos do contêiner

Para obter detalhes, incluindo guias de início rápido, consulte a [documentação dos contêineres do Windows](/virtualization/windowscontainers/index).

### <a name="windows-powershell-direct-new"></a>Novo Windows PowerShell Direct \(\)

Isso lhe dá uma maneira de executar comandos do Windows PowerShell em uma máquina virtual a partir do host. O Windows PowerShell Direct é executado entre o host e a máquina virtual. Isso significa que ele não requer requisitos de rede ou firewall e funciona independentemente da sua configuração de gerenciamento remoto.

O Windows PowerShell Direct é uma alternativa para as ferramentas existentes que os administradores do Hyper-V usam para se conectar a uma máquina virtual em um host Hyper-V:

-   Ferramentas de gerenciamento remoto como o PowerShell ou a Área de Trabalho Remota

-   Conexão de máquina virtual do Hyper-V (VMConnect)

Essas ferramentas funcionam bem, mas têm compensações: VMConnect é confiável, mas pode ser difícil de automatizar. O PowerShell remoto é poderoso, mas pode ser difícil de configurar e manter. Essas compensações podem se tornar mais importantes à medida que sua implantação do Hyper-V cresce. O Windows PowerShell Direct aborda isso fornecendo uma experiência avançada de script e automação que é tão simples quanto usar o VMConnect.

Para obter os requisitos e as instruções, consulte [gerenciar máquinas virtuais do Windows com o PowerShell Direct](manage/Manage-Windows-virtual-machines-with-PowerShell-Direct.md).
