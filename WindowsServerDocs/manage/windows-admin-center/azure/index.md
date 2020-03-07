---
title: Como conectar o Windows Server aos serviços híbridos do Azure
description: É possível estender as implantações locais do Windows Server para a nuvem usando os serviços híbridos do Azure.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server
ms.date: 05/31/2019
ms.openlocfilehash: b82d2eaa9283d99993102f1656262e2eda86cfff
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371748"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Como conectar o Windows Server aos serviços híbridos do Azure

É possível estender as implantações locais do Windows Server para a nuvem usando os serviços híbridos do Azure. Esses serviços de nuvem fornecem uma matriz de funções úteis, tanto para a extensão local no Azure quanto para o gerenciamento central do Azure.

![Diagrama mostrando a seta do local para a nuvem para estender o local para o Azure e a seta da nuvem para o local para gerenciamento central com o Azure](../media/azure-services/hybrid-framing.png)

Usando os serviços híbridos do Azure no Windows Admin Center, é possível:

- [Proteger máquinas virtuais e usar backup baseado em nuvem e recuperação de desastre (HA/DR)](#back-up-and-protect-your-on-premises-servers-and-vms).  
- [Estender a capacidade local com armazenamento e computação no Azure e simplificar a conectividade de rede para o Azure](#extend-on-premises-capacity-with-azure).
- [Centralizar o monitoramento, a governança, a configuração e a segurança em seus aplicativos, rede e infraestrutura com a ajuda dos serviços de gerenciamento do Azure de nuvem inteligente](#centrally-manage-your-hybrid-environment-from-azure).  

Embora seja possível configurar a maioria dos serviços híbridos do Azure baixando um aplicativo e realizando alguma configuração manual, muitos são integrados diretamente ao Windows Admin Center para oferecer uma experiência de instalação simplificada e uma exibição dos serviços centrada no servidor. O Windows Admin Center também fornece hiperlinks inteligentes convenientes para o portal do Azure para ver recursos conectados do Azure, bem como uma exibição centralizada de seu ambiente híbrido.

## <a name="discover-integrated-services-in-the-azure-hybrid-services-tool"></a>Descobrir serviços integrados na ferramenta de serviços híbridos do Azure

A ferramenta de serviços híbridos do Azure no [Windows Admin Center](../overview.md) consolida todos os serviços do Azure integrados em um hub centralizado em que você pode descobrir com facilidade todos os serviços do Azure disponíveis que agregam valor ao seu ambiente local ou híbrido.  

![Captura de tela do Windows Admin Center mostrando a ferramenta de Serviços Híbridos do Azure](../media/azure-services/ahs-discover.png)

Se você se conectar a um servidor com os serviços do Azure já habilitados, a ferramenta de serviços híbridos do Azure funcionará como um único painel para ver todos os serviços habilitados nesse servidor. É possível obter facilmente a ferramenta relevante no Windows Admin Center, iniciar no portal do Azure para obter um gerenciamento mais aprofundado desses serviços do Azure ou ler mais com a documentação ao seu alcance.  

![Captura de tela do Windows Admin Center mostrando os serviços do Azure que já foram instalados no servidor](../media/azure-services/ahs-dayN.png)

Na ferramenta de serviços híbridos do Azure, é possível:

- Fazer backup do Windows Server do Windows Admin Center com o [Backup do Azure](azure-backup.md)
- Proteger suas máquinas virtuais do Hyper-V do Windows Admin Center com o [Azure Site Recovery](azure-site-recovery.md)
- Sincronizar seu servidor de arquivos com a nuvem usando a [Sincronização de Arquivos do Azure](azure-file-sync.md)
- Gerenciar as atualizações do sistema operacional para todos os seus servidores Windows, locais ou na nuvem, com o [Gerenciamento de Atualizações do Azure](azure-update-management.md)
- Monitorar os servidores, locais ou na nuvem, e configurar alertas com o [Azure Monitor](azure-monitor.md)
- Aplicar políticas de governança a seus servidores locais por meio do Azure Policy usando o [Azure Arc para servidores](https://docs.microsoft.com/azure/azure-arc/servers/overview)
- Proteger seus servidores e obter proteção avançada contra ameaças com a [Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration)
- Conectar seus servidores locais com uma Rede Virtual do Azure com o [Adaptador de Rede do Azure](https://aka.ms/WACNetworkAdapter)
- Fazer com que as VMs do Azure se pareçam com sua rede local com a [Rede Estendida do Azure](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409)

## <a name="back-up-and-protect-your-on-premises-servers-and-vms"></a>Fazer backup e proteger seus servidores e VMs locais

- **Fazer backup de seus servidores Windows com o [Backup do Azure](https://docs.microsoft.com/azure/backup/backup-overview)**  
É possível fazer backup de seus servidores Windows no Azure, ajudando a proteger você contra corrupção, ransomware e exclusões acidentais ou mal-intencionadas.  
Para mais informações, confira [Fazer backup dos seus servidores com o Backup do Azure](azure-backup.md).

- **Proteger suas máquinas virtuais Hyper-V com o [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
É possível replicar cargas de trabalho em execução em VMs para que a infraestrutura crítica de negócios esteja protegida em caso de desastre. O Windows Admin Center simplifica a instalação e o processo de replicação de máquinas virtuais nos servidores Hyper-V ou clusters, tornando mais fácil reforçar a resiliência do ambiente com o serviço de recuperação de desastre do Azure Site Recovery.  
Para mais informações, confira [Proteger suas VMs com o Azure Site Recovery e com o Windows Admin Center](azure-site-recovery.md).

- **Usar a replicação síncrona ou assíncrona baseada em blocos para uma VM no Azure usando a [Réplica de Armazenamento](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-overview)**  
Você pode configurar a replicação baseada em bloco ou em volume em um nível de servidor para servidor usando a Réplica de Armazenamento para um servidor secundário ou uma VM. O Windows Admin Center permite que você crie uma VM do Azure especificamente para seu destino de replicação, ajudando você a configurar corretamente o armazenamento em uma nova VM do Azure.  
Para obter mais informações, consulte [Replicação de servidor para servidor com a Réplica de Armazenamento](https://docs.microsoft.com/windows-server/storage/storage-replica/storage-replica-ui).  

## <a name="extend-on-premises-capacity-with-azure"></a>Estender a capacidade local com o Azure

### <a name="extend-storage-capacity"></a>Estender a capacidade de armazenamento

- **Sincronizar seu servidor de arquivos com a nuvem usando a [Sincronização de Arquivos do Azure](https://aka.ms/afs)**  
Sincronize arquivos neste servidor com os compartilhamentos de arquivo do Azure. Mantenha todos os seus arquivos locais ou use a camada de nuvem para liberar espaço e armazene em cache apenas os arquivos usados com mais frequência no servidor, dispondo em camadas os dados frios na nuvem. O backup dos dados na nuvem pode ser realizado, acabando com a necessidade de se preocupar com o backup do servidor local. Além disso, a sincronização de vários sites pode manter um conjunto de arquivos em sincronia entre vários servidores.
Para obter mais informações, consulte [Sincronizar seu servidor de arquivos com a nuvem usando a Sincronização de Arquivos do Azure](azure-file-sync.md).

- **Migrar o armazenamento para uma VM no Azure usando o [Serviço de Migração de armazenamento](https://docs.microsoft.com/windows-server/storage/storage-migration-service/overview)**  
Use a ferramenta passo a passo para inventariar dados em servidores Windows e Linux e, em seguida, transferir os dados para uma nova VM do Azure. O Windows Admin Center pode criar uma VM do Azure para o trabalho que tem o tamanho e a configuração corretos para receber os dados do seu servidor de origem.  
Para obter mais informações, consulte [Usar o Serviço de Migração de Armazenamento para migrar um servidor](https://docs.microsoft.com/windows-server/storage/storage-migration-service/migrate-data).

### <a name="extend-compute-capacity"></a>Estender a capacidade de computação

- **Criar uma máquina virtual do Azure sem sair do Windows Admin Center**  
Na página *Todas as Conexões* no Windows Admin Center, vá para **Adicionar** e selecione **Criar** em **VM do Azure**. Você pode até ingressar no domínio de sua VM do Azure e configurar o armazenamento de dentro desta ferramenta de criação passo a passo.

- **Aproveitar o Azure para atingir o quorum em seu cluster de failover com a [Testemunha em Nuvem](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness)**  
Em vez de investir em hardware adicional para obter quorum em um cluster de dois nós, é possível usar uma conta de armazenamento do Azure para servir como a testemunha de cluster para seu cluster do Azure Stack HCI ou outro cluster de failover.  
Para saber mais, confira [Implantar uma testemunha de nuvem para um Cluster de Failover](https://docs.microsoft.com/windows-server/failover-clustering/deploy-cloud-witness).  

### <a name="simplify-network-connectivity-between-your-on-premises-and-azure-networks"></a>Simplifique a conectividade de rede entre suas redes locais e do Azure

- **Conectar seus servidores locais com uma Rede Virtual do Azure com o [Adaptador de Rede do Azure](https://aka.ms/WACNetworkAdapter)**  
Deixe o Windows Admin Center simplificar a configuração de uma VPN ponto a site de um servidor local para uma rede virtual do Azure.  

- **Fazer com que as VMs do Azure se pareçam com sua rede local com a [Rede Estendida do Azure](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409)**  
O Windows Admin Center pode configurar uma VPN site a site e estender seus endereços IP locais para sua vNet do Azure a fim de permitir que você migre cargas de trabalho com mais facilidade para o Azure sem perder dependências em endereços IP.

## <a name="centrally-manage-your-hybrid-environment-from-azure"></a>Gerenciar centralmente seu ambiente híbrido do Azure

- **Monitorar e receber alertas de email para todos os servidores em seu ambiente com o [Azure Monitor para Máquinas Virtuais](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
É possível usar o Azure Monitor, também conhecido como Insights de Máquinas Virtuais, para monitorar a integridade e os eventos do servidor, criar alertas de email, obter uma exibição consolidada do desempenho do servidor em seu ambiente e visualizar aplicativos, sistemas e serviços conectados a um determinado servidor. O Windows Admin Center também pode configurar alertas de email padrão para o desempenho de integridade do servidor e eventos de integridade do cluster.  
Para mais informações, confira [Conectar seus servidores ao Azure Monitor e configurar notificações por email](azure-monitor.md).

- **Gerenciar centralmente atualizações do sistema operacional para todos os seus servidores Windows com o [Gerenciamento de Atualizações do Azure](https://docs.microsoft.com/azure/automation/automation-update-management)**  
É possível gerenciar atualizações e patches para vários servidores e VMs em um único lugar, em vez de fazê-lo por servidor. Com o Gerenciamento de Atualizações do Azure, é possível avaliar rapidamente o status de atualizações disponíveis, agendar a instalação de atualizações necessárias e examinar os resultados de implantação para verificar atualizações que foram aplicadas com êxito. Isso será possível se seus servidores forem VMs do Azure, hospedadas por outros provedores de nuvem ou locais.  
Para mais informações, confira [Configurar servidores para o Gerenciamento de Atualizações do Azure](azure-update-management.md).

- **Melhorar a postura de segurança e obter proteção avançada contra ameaças com a [Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/security-center-intro)**  
A Central de Segurança do Azure é um sistema de gerenciamento de segurança de infraestrutura unificado que fortalece a postura de segurança de seus data centers e fornece proteção avançada contra ameaças em suas cargas de trabalho híbridas na nuvem, estejam elas no Azure ou não, bem como no local. Com o Windows Admin Center, é possível configurar e conectar facilmente seus servidores à Central de Segurança do Azure.  
Para obter mais informações, consulte [Integrar a Central de Segurança do Azure com o Windows Admin Center (versão prévia)](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration).  

- **Aplicar políticas e garantir a conformidade em seu ambiente híbrido com o [Azure Arc para servidores](https://docs.microsoft.com/azure/azure-arc/servers/overview) e o [Azure Policy](https://docs.microsoft.com/azure/governance/policy/overview)**  
Faça inventário, organize e gerencie servidores locais do Azure. É possível administrar servidores usando a política do Azure, controlar o acesso usando o RBAC (controle de acesso baseado em função) e habilitar serviços de gerenciamento adicionais do Azure.  

## <a name="clusters-versus-stand-alone-servers-and-vms"></a>Clusters versus servidores e VMs autônomos

Os serviços híbridos do Azure funcionam com os Servidores do Windows nas seguintes configurações:

- Servidores físicos e VMs (máquinas virtuais) autônomos
- Clusters, incluindo clusters hiperconvergentes certificados pelo [Azure Stack HCI](../../../azure-stack-hci/index.md) e por programas [WSSD (definidos pelo software do Windows Server)](https://www.microsoft.com/cloud-platform/software-defined-datacenter)

### <a name="services-for-stand-alone-servers-and-vms"></a>Serviços para servidores e VMs autônomos

Esta é a lista completa dos serviços do Azure que oferecem uma funcionalidade a servidores e VMs autônomos:

- Fazer backup do Windows Server do Windows Admin Center com o [Backup do Azure](azure-backup.md)
- Proteger suas máquinas virtuais do Hyper-V do Windows Admin Center com o [Azure Site Recovery](azure-site-recovery.md)
- Sincronizar seu servidor de arquivos com a nuvem usando a [Sincronização de Arquivos do Azure](azure-file-sync.md)
- Gerenciar as atualizações do sistema operacional para todos os seus servidores Windows, locais ou na nuvem, com o [Gerenciamento de Atualizações do Azure](azure-update-management.md)
- Monitorar os servidores, locais ou na nuvem, e configurar alertas com o [Azure Monitor](azure-monitor.md)
- Aplicar políticas de governança a seus servidores locais por meio do Azure Policy usando o [Azure Arc para servidores](https://docs.microsoft.com/azure/azure-arc/servers/overview)
- Proteger seus servidores e obter proteção avançada contra ameaças com a [Central de Segurança do Azure](https://docs.microsoft.com/azure/security-center/windows-admin-center-integration)
- Conectar seus servidores locais com uma Rede Virtual do Azure com o [Adaptador de Rede do Azure](https://aka.ms/WACNetworkAdapter)
- Fazer com que as VMs do Azure se pareçam com sua rede local com a [Rede Estendida do Azure](https://go.microsoft.com/fwlink/?linkid=2109517&clcid=0x409)

### <a name="services-for-clusters"></a>Serviços para clusters

Estes são os serviços do Azure que fornecem funcionalidade para clusters como um todo:

- [Monitorar um cluster hiperconvergente com o Azure Monitor](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Proteger suas VMs com o Azure Site Recovery](azure-site-recovery.md)
- [Implantar uma testemunha de nuvem do cluster](../../../failover-clustering/deploy-cloud-witness.md)  

## <a name="other-azure-integrated-abilities-of-windows-admin-center"></a>Outras capacidades integradas do Azure no Windows Admin Center

- **[Adicionar conexões de VM do Azure](manage-azure-vms.md) no Windows Admin Center**  
É possível usar o Windows Admin Center para gerenciar suas VMs do Azure, bem como seus computadores locais. Ao configurar o gateway do Windows Admin Center para se conectar à VNet do Azure, é possível gerenciar máquinas virtuais no Azure usando as ferramentas consistentes e simplificadas que o Windows Admin Center oferece.  
Para mais informações, confira [Configurar o Windows Admin Center para gerenciar as VMs no Azure](manage-azure-vms.md).

- **Adicionar uma camada de segurança ao Windows Admin Center adicionando a [autenticação do](https://azure.microsoft.com/services/active-directory/) Azure AD (Active Directory)**  
É possível adicionar uma camada adicional de segurança ao Windows Admin Center exigindo que os usuários se autentiquem usando identidades do Azure AD (Active Directory) para acessar o gateway. A autenticação do Azure AD também permite que você aproveite os recursos de segurança do Azure AD, como acesso condicional e autenticação multifator.  
Para obter mais informações, consulte [Configurar autenticação do Azure AD para o Windows Admin Center](../configure/user-access-control.md#azure-active-directory).  

- **Gerenciar recursos do Azure diretamente por meio do [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview) inserido no Windows Admin Center**  
Aproveite o Azure Cloud Shell para obter uma experiência do Bash ou do PowerShell no Windows Admin Center e ter acesso fácil às tarefas de gerenciamento do Azure.  
Para obter mais informações, consulte [Visão geral do Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/overview).


## <a name="see-also"></a>Consulte também

- [Conectar o Windows Admin Center ao Azure](azure-integration.md)
- [Implantar o Windows Admin Center no Azure](deploy-wac-in-azure.md)
