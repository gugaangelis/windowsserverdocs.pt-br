---
title: Conectando-se o Windows Server para os serviços do Azure híbrido
description: Você pode estender as implantações locais do Windows Server na nuvem usando serviços da híbrido do Azure.
ms.technology: manage
ms.topic: article
author: jasongerend
ms.author: jgerend
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.date: 04/12/2019
ms.openlocfilehash: 625bd4b79d277dfaa81767cd781c2ba1316d637e
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296853"
---
# Conectando-se o Windows Server para os serviços do Azure híbrido

>Aplica-se a: Windows Server 2019, Windows Server 2016

Você pode estender as implantações locais do Windows Server na nuvem usando serviços da híbrido do Azure. Esses serviços em nuvem fornecem uma matriz de funções úteis, incluindo o seguinte:

- Proteger máquinas virtuais e use baseado em nuvem backup e recuperação de desastres (alta disponibilidade/RD) com o Azure Site Recovery. 
- Acompanhe o que está acontecendo em todos os aplicativos, rede e infraestrutura com a Ajuda de análise avançada e no Azure Monitor de aprendizado de máquina. 
- Simplifica a conectividade de rede para o Azure com o adaptador de rede do Azure.
- Máquinas virtuais manter atualizado com o gerenciamento de atualização do Azure.

Serviços do Azure híbrido trabalhar com servidores do Windows as seguintes configurações:

- Autônomos servidores físicos e máquinas virtuais (VMs)
- Clusters, incluindo clusters hiperconvergentes certificadas pela [HCI de pilha do Azure](../../../azure-stack-hci/index.md)e programas de [Construção (WSSD)](https://www.microsoft.com/en-us/cloud-platform/software-defined-datacenter)

Enquanto você pode configurar o Azure mais serviços de híbrido usando o portal do Azure e um download ou duas, muitos são integrados diretamente no Centro de administração do Windows para fornecer uma experiência de instalação simplificada e uma exibição centrada no servidor dos serviços.

## Ferramenta de serviços do Azure híbrido

A ferramenta de serviços híbrido do Azure no [Windows Admin Center](../understand/windows-admin-center.md) consolida todos os serviços do Azure integrados em um hub centralizado onde você pode descobrir facilmente todos os serviços Azure disponíveis que agregam valor ao seu local ou híbrido ambiente. 

![Captura de tela do Windows Admin Center mostrando a ferramenta de serviços do Azure híbrido](../media/azure-services/ahs-discover.png)

Se você se conectar a um servidor com serviços do Azure já está habilitados, a ferramenta de serviços do Azure híbrido serve como um único painel para ver todos os serviços habilitados no servidor. Você pode facilmente acessar a ferramenta relevante no Windows Admin Center, iniciar portal do Azure para gerenciamento mais profundo desses serviços do Azure ou leitura mais com documentação ao seu alcance. 

![Captura de tela do Windows Admin Center mostrando os serviços do Azure que já estão instalados no servidor](../media/azure-services/ahs-dayN.png)

Na ferramenta de serviços do Azure híbrido, você pode:
- Backup do Windows Server do Windows Admin Center com [Backup do Azure](azure-backup.md)
- Proteger suas máquinas de virtuais do Hyper-V do Windows Admin Center com o [Azure Site Recovery](azure-site-recovery.md)
- Sincronizar seu servidor de arquivos com a nuvem, usando [A sincronização de arquivo do Azure](azure-file-sync.md)
- Gerenciar atualizações do sistema operacional para todos os servidores do Windows, no local ou na nuvem, com o [Gerenciamento de atualização do Azure](azure-update-management.md)
- Monitorar servidores, no local ou na nuvem e configurar alertas com o [Monitor do Azure](azure-monitor.md)
- Conecte seus servidores locais para uma rede Virtual do Azure com [O adaptador de rede do Azure](https://aka.ms/WACNetworkAdapter)

## Serviços para VMs e servidores autônomos

Esta é a lista completa de serviços do Azure que fornecem funcionalidade para servidores autônomos e VMs:

- **(Novo) Sincronizar seu servidor de arquivos com a nuvem usando [A sincronização de arquivo do Azure](https://aka.ms/afs)**  
Sincronizar arquivos neste servidor com os compartilhamentos de arquivo do Azure. Manter todos os arquivos locais ou uso somente em nuvem cache e hierarquia mais usados com frequência arquivos no servidor, armazenamento hierárquico frios dados na nuvem. Dados na nuvem podem ser backup, eliminando a necessidade de se preocupar sobre backup do servidor local. Além disso, sincronização de vários locais pode manter um conjunto de arquivos em sincronia com vários servidores.

- **Adicionar uma camada de segurança ao Windows Admin Center, incluindo autenticação do [Azure Active Directory (Azure AD)](https://azure.microsoft.com/services/active-directory/)**  
Você pode adicionar uma camada adicional de segurança para o Windows Admin Center por exigir que os usuários são autenticados usando identidades do Azure Active Directory (Azure AD) para acessar o gateway. Autenticação do Azure AD também permite que você tirar proveito dos recursos de segurança do Azure AD como o acesso condicional e a autenticação multifator.  
Para obter mais informações, consulte [autenticação de configurar o Azure AD para o Windows Admin Center.](../configure/user-access-control.md#azure-active-directory)  

- **Proteger suas máquinas de virtuais do Hyper-V com o [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview)**  
Você pode replicar cargas de trabalho em execução nas VMs para proteger sua infraestrutura críticos de negócios em caso de desastre. Windows Admin Center simplifica a instalação e o processo de replicação de suas máquinas virtuais nos servidores do Hyper-V ou clusters, tornando mais fácil reforçar a resiliência do seu ambiente com o serviço de recuperação de desastres do Azure Site Recovery.  
Para obter mais informações, consulte [proteger suas VMs com o Azure Site Recovery e o Windows Admin Center](azure-site-recovery.md).

- **Fazer backup de seus servidores do Windows com o [Backup do Azure](https://docs.microsoft.com/azure/backup/backup-overview)**  
Você pode fazer backup de seus servidores do Windows Azure, ajudando a proteger você contra ransomware, corrupção e exclusões acidentais ou mal-intencionados.  
Para obter mais informações, consulte [fazer backup de seus servidores com o Backup do Azure](azure-backup.md).

- **Monitorar e obter alertas de email para todos os servidores em seu ambiente com o [Monitor do Azure para máquinas virtuais](https://docs.microsoft.com/azure/azure-monitor/insights/vminsights-overview)**  
Você pode usar o Monitor do Azure, também conhecido como máquinas virtuais Insights, para monitorar a integridade do servidor e eventos, criar alertas de email, obtenha uma visão consolidada de desempenho do servidor em seu ambiente e visualizar aplicativos, sistemas, e serviços conectados a um determinado servidor.  
Para obter mais informações, consulte [conecte seus servidores ao Monitor do Azure e configurar notificações por email](azure-monitor.md).

- **Gerenciar centralmente as atualizações do sistema operacional para todos os servidores do Windows com o [Gerenciamento de atualização do Azure](https://docs.microsoft.com/azure/automation/automation-update-management)**  
Você pode gerenciar atualizações e patches para vários servidores e VMs em um único local, em vez de em uma base por servidor. Com o gerenciamento de atualização do Azure, você pode rapidamente avaliar o status das atualizações disponíveis, agendar a instalação de atualizações necessárias e analisar os resultados de implantação para verificar as atualizações que se aplicam com êxito. Isso é possível se os servidores são VMs do Azure, hospedado por outros provedores de nuvem, ou local.  
Para obter mais informações, consulte [Configurar servidores para gerenciamento de atualização do Azure](azure-update-management.md).

- **Conecte seus servidores locais para uma rede Virtual do Azure com um [Adaptador de rede do Azure](https://aka.ms/WACNetworkAdapter)**  
Você pode adicionar um adaptador de rede do Azure para seus servidores locais para ajudar você a se conectar com segurança o servidor a uma rede Virtual do Azure.  
Para obter mais informações, consulte [Configurar um ponto ao site conexão de VPN entre um local Windows Server e uma rede Virtual do Azure](https://aka.ms/WACNetworkAdapter).

- **Gerenciar máquinas virtuais de IaaS do Azure com o [Windows Admin Center](manage-azure-vms.md)**  
Você pode usar o Windows Admin Center para gerenciar suas VMs do Azure, bem como máquinas locais. Configurando seu gateway do Windows Admin Center para se conectar ao seu VNet do Azure, você pode gerenciar máquinas virtuais no Azure usando as ferramentas consistentes e simplificadas que fornece o Windows Admin Center. Para obter mais informações, consulte [Configurar o Windows Admin Center para gerenciar VMs no Azure](manage-azure-vms.md).

## Serviços para clusters

Estes são os serviços do Azure que fornecem funcionalidade para clusters como um todo (eles não são todos integrados ao Windows Admin Center ainda):

- [Monitorar um cluster hiperconvergente com o sistema do Azure](../../../storage/storage-spaces/configure-azure-monitor.md)
- [Proteger suas VMs com o Azure Site Recovery](azure-site-recovery.md)
- [Implantar uma testemunha de nuvem do cluster](../../../failover-clustering/deploy-cloud-witness.md)

## Ver também

- [Conectar-se o Centro de administração do Windows Azure](azure-integration.md)
- [Implantar o Windows Admin Center no Azure](deploy-wac-in-azure.md)