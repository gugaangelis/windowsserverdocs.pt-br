---
title: Como conectar o Windows Server aos serviços híbridos do Azure
description: É possível estender as implantações locais do Windows Server para a nuvem usando os serviços híbridos do Azure.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 05/31/2019
ms.openlocfilehash: 460399a57bc229b44d37a9fdd1e4938bf9e7d6ac
ms.sourcegitcommit: fe621b72d45d0259bac1d5b9031deed3dcbed29d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2019
ms.locfileid: "66455367"
---
# <a name="connecting-windows-server-to-azure-hybrid-services"></a>Como conectar o Windows Server aos serviços híbridos do Azure

>Aplica-se a: Windows Server 2019, Windows Server 2016, Windows Server (canal semestral)

É possível estender as implantações locais do Windows Server para a nuvem usando os serviços híbridos do Azure. Esses serviços de nuvem oferecem uma matriz de funções úteis, incluindo o seguinte:

- Proteger máquinas virtuais e usar backup baseado em nuvem e recuperação de desastre (HA/DR) com o Azure Site Recovery. 
- Acompanhar o que está acontecendo em seus aplicativos, na rede e na infraestrutura com a ajuda de análises avançadas e aprendizado de máquina no Azure Monitor. 
- Simplificar a conectividade de rede no Azure com Adaptador de Rede do Azure.
- Manter as máquinas virtuais atualizadas com o Gerenciamento de Atualizações do Azure.

Os serviços híbridos do Azure funcionam com os Servidores do Windows nas seguintes configurações:

- Servidores físicos e VMs (máquinas virtuais) autônomos
- Clusters, incluindo clusters hiperconvergentes certificados pelo [Azure Stack HCI](https://docs.microsoft.com/azure-stack/operator/azure-stack-hci-overview) e por programas [WSSD (definidos pelo software do Windows Server)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)

Embora seja possível configurar a maioria dos serviços híbridos do Azure usando o portal do Azure e um download ou dois, muitos são integrados diretamente ao Windows Admin Center para oferecer uma experiência de instalação simplificada e uma exibição dos serviços centrada no servidor.

## <a name="azure-hybrid-services-tool"></a>Ferramenta de serviços híbridos do Azure

A ferramenta de serviços híbridos do Azure no [Windows Admin Center](../understand/windows-admin-center.md) consolida todos os serviços do Azure integrados em um hub centralizado em que você pode descobrir com facilidade todos os serviços do Azure disponíveis que agregam valor ao seu ambiente local ou híbrido. 

![Captura de tela do Windows Admin Center mostrando a ferramenta de Serviços Híbridos do Azure](../media/azure-services/ahs-discover.png)

Se você se conectar a um servidor com os serviços do Azure já habilitados, a ferramenta de serviços híbridos do Azure funcionará como um único painel para ver todos os serviços habilitados nesse servidor. É possível obter facilmente a ferramenta relevante no Windows Admin Center, iniciar no portal do Azure para obter um gerenciamento mais aprofundado desses serviços do Azure ou ler mais com a documentação ao seu alcance. 

![Captura de tela do Windows Admin Center mostrando os serviços do Azure que já foram instalados no servidor](../media/azure-services/ahs-dayN.png)

Na ferramenta de serviços híbridos do Azure, é possível:
- Fazer backup do Windows Server do Windows Admin Center com o [Backup do Azure](azure-backup.md)
- Proteger suas máquinas virtuais do Hyper-V do Windows Admin Center com o [Azure Site Recovery](azure-site-recovery.md)
- Sincronizar seu servidor de arquivos com a nuvem usando a [Sincronização de Arquivos do Azure](azure-file-sync.md)
- Gerenciar as atualizações do sistema operacional para todos os seus servidores Windows, locais ou na nuvem, com o [Gerenciamento de Atualizações do Azure](azure-update-management.md)
- Monitorar os servidores, locais ou na nuvem, e configurar alertas com o [Azure Monitor](azure-monitor.md)
- Conectar seus servidores locais com uma Rede Virtual do Azure com o [Adaptador de Rede do Azure](https://aka.ms/WACNetworkAdapter)

## <a name="services-for-stand-alone-servers-and-vms"></a>Serviços para servidores e VMs autônomos

Esta é a lista completa dos serviços do Azure que oferecem uma funcionalidade a servidores e VMs autônomos:

- **(Novo) Sincronizar seu servidor de arquivos com a nuvem usando a [Sincronização de Arquivos do Azure](https://aka.ms/afs)**  
Sincronize arquivos neste servidor com os compartilhamentos de arquivo do Azure. Mantenha todos os seus arquivos locais ou use a camada de nuvem e armazene em cache apenas os arquivos usados com mais frequência no servidor, dispondo em camadas os dados frios na nuvem. O backup dos dados na nuvem pode ser realizado, acabando com a necessidade de se preocupar com o backup do servidor local. Além disso, a sincronização de vários sites pode manter um conjunto de arquivos em sincronia entre vários servidores.

- **Adicionar uma camada de segurança ao Windows Admin Center adicionando a [autenticação do](https://azure.microsoft.com/services/active-directory/) Azure AD (Active Directory)**  
É possível adicionar uma camada adicional de segurança ao Windows Admin Center exigindo que os usuários se autentiquem usando identidades do Azure AD (Active Directory) para acessar o gateway. A autenticação do Azure AD também permite que você aproveite os recursos de segurança do Azure AD, como acesso condicional e autenticação multifator.  
Para mais informações, confira [Configurar autenticação do Azure AD para o Windows Admin Center.](../configure/user-access-control.md#azure-active-directory)  

- **Proteger suas máquinas virtuais Hyper-V com o [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
É possível replicar cargas de trabalho em execução em VMs para que a infraestrutura crítica de negócios esteja protegida em caso de desastre. O Windows Admin Center simplifica a instalação e o processo de replicação de suas máquinas virtuais nos servidores Hyper-V ou clusters, tornando mais fácil reforçar a resiliência do seu ambiente com o serviço de recuperação de desastre do Azure Site Recovery.  
Para mais informações, confira [Proteger suas VMs com o Azure Site Recovery e com o Windows Admin Center](azure-site-recovery.md).

- **Fazer backup de seus servidores Windows com o [Backup do Azure](https://docs.microsoft.com/azure/backup/backup-overview)**  
É possível fazer backup de seus servidores Windows no Azure, ajudando a proteger você contra corrupção, ransomware e exclusões acidentais ou mal-intencionadas.  
Para mais informações, confira [Fazer backup dos seus servidores com o Backup do Azure](azure-backup.md).

- **Monitorar e receber alertas de email para todos os servidores em seu ambiente com o [Azure Monitor para Máquinas Virtuais](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
É possível usar o Azure Monitor, também conhecido como Insights de Máquinas Virtuais, para monitorar a integridade e os eventos do servidor, criar alertas de email, obter uma exibição consolidada do desempenho do servidor em seu ambiente e visualizar aplicativos, sistemas e serviços conectados a um determinado servidor.  
Para mais informações, confira [Conectar seus servidores ao Azure Monitor e configurar notificações por email](azure-monitor.md).

- **Gerenciar centralmente atualizações do sistema operacional para todos os seus servidores Windows com o [Gerenciamento de Atualizações do Azure](https://docs.microsoft.com/azure/automation/automation-update-management)**  
É possível gerenciar atualizações e patches para vários servidores e VMs em um único lugar, em vez de fazê-lo por servidor. Com o Gerenciamento de Atualizações do Azure, é possível avaliar rapidamente o status de atualizações disponíveis, agendar a instalação de atualizações necessárias e examinar os resultados de implantação para verificar atualizações que foram aplicadas com êxito. Isso será possível se seus servidores forem VMs do Azure, hospedadas por outros provedores de nuvem ou locais.  
Para mais informações, confira [Configurar servidores para o Gerenciamento de Atualizações do Azure](azure-update-management.md).

- **Conectar seus servidores locais a uma Rede Virtual do Azure com um [Adaptador de Rede do Azure](https://aka.ms/WACNetworkAdapter)**  
É possível adicionar um Adaptador de Rede do Azure aos seus servidores locais para ajudar você a conectar o servidor com segurança a uma Rede Virtual do Azure.  
Para mais informações, confira [Configurar uma conexão VPN de ponto a site entre um Windows Server local e uma Rede Virtual do Azure](https://aka.ms/WACNetworkAdapter).

- **Gerenciar máquinas virtuais IaaS do Azure com o [Windows Admin Center](manage-azure-vms.md)**  
É possível usar o Windows Admin Center para gerenciar suas VMs do Azure, bem como seus computadores locais. Ao configurar o gateway do Windows Admin Center para se conectar à VNet do Azure, é possível gerenciar máquinas virtuais no Azure usando as ferramentas consistentes e simplificadas que o Windows Admin Center oferece. Para mais informações, confira [Configurar o Windows Admin Center para gerenciar as VMs no Azure](manage-azure-vms.md).

## <a name="services-for-clusters"></a>Serviços para clusters

Esses são os serviços do Azure que oferecem funcionalidades aos clusters como um todo (todas elas não foram integradas ao Windows Admin Center ainda):

- [Monitorar um cluster hiperconvergente com o Azure Monitor](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Proteger suas VMs com o Azure Site Recovery](azure-site-recovery.md)
- [Implantar uma testemunha de nuvem do cluster](../../../failover-clustering/deploy-cloud-witness.md)

## <a name="see-also"></a>Consulte também

- [Conectar o Windows Admin Center ao Azure](azure-integration.md)
- [Implantar o Windows Admin Center no Azure](deploy-wac-in-azure.md)