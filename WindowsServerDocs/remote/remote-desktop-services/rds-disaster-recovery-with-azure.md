---
title: Configurar a recuperação de desastre para o RDS usando o Azure Disaster Recovery
description: Saiba como usar o Azure Disaster Recovery para recuperação de desastre em uma implantação do RDS
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 06/12/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 514262fde3b433baf89fe8f5a0cf8b04ef267354
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71387539"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>Configurar a recuperação de desastre para o RDS usando o Azure Site Recovery

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Você pode usar o Azure Site Recovery para criar uma solução de recuperação de desastre para sua implantação dos Serviços de Área de Trabalho Remota. 

A [Azure Site Recovery](/azure/site-recovery/site-recovery-overview) é um serviço baseado no Azure que fornece recursos de recuperação de desastre orquestrando a replicação, o failover e a recuperação de máquinas virtuais. O Azure Site Recovery dá suporte a uma série de tecnologias de replicação para replicar de forma consistente, proteger e executar o failover contínuo de máquinas virtuais e aplicativos para nuvens privadas/públicas do host. 

Use as informações a seguir para criar e validar a solução de recuperação de desastre.

## <a name="disaster-recovery-deployment-options"></a>Opções de implantação de recuperação de desastre

Você pode implantar o RDS em servidores físicos ou máquinas virtuais que executam Hyper-V ou VMWare. O Azure Site Recovery pode proteger implantações locais e virtuais em um site secundário ou no Azure. A tabela a seguir mostra das diferentes implantações de RDS com suporte em cenários de recuperação de desastre site a site e site ao Azure.

| Tipo de implantação                          | Hyper-V site a site | Hyper-V site ao Azure | VMWare site ao Azure | Físico site ao Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| Área de trabalho virtual em pool (não gerenciada)       |Sim|Não|Não|Não |
| Área de trabalho virtual em pool (gerenciada, sem UPD) | Sim|Não|Não|Não|
| Sessões de RemoteApps e área de trabalho (sem UPD) | Sim|Sim|Sim|Sim  |

## <a name="prerequisites"></a>Pré-requisitos

Antes de configurar o Azure Site Recovery para sua implantação, certifique-se de que cumprir os seguintes requisitos:

- Criar uma [implantação de RDS local](rds-deploy-infrastructure.md).
- Adicione o [Cofre dos Serviços do Azure Site Recovery](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault) à sua assinatura do Microsoft Azure.
- Se você pretende usar o Azure como seu site de recuperação, execute a [ferramenta de Avaliação de Prontidão da Máquina Virtual do Azure](https://azure.microsoft.com/downloads/vm-readiness-assessment/) em suas VMs para garantir que sejam compatíveis com as VMs do Azure e os Serviços do Azure Site Recovery.
 
## <a name="implementation-checklist"></a>Lista de verificação de implementação

Abordaremos as diferentes etapas para habilitar os Serviços do Azure Site Recovery para sua implantação do RDS com mais detalhes, mas estas são as etapas gerais da implementação.

| **Etapa 1 – Configurar VMs para recuperação de desastres**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V – baixe o Provedor do Microsoft Azure Site Recovery. Instale-o em seu servidor de VMM ou host Hyper-V. Consulte [Pré-requisitos para replicação no Azure usando o Azure Site Recovery](/azure/site-recovery/site-recovery-prereq) para obter informações.                                                                                                                             |
| VMWare — configure o servidor de proteção, o servidor de configuração e os servidores de destino mestre                                                                                                                                                      |
| **Etapa 2 – Preparar seus recursos**                                                                                                                                                                                                           |
| Adicione uma [conta do Armazenamento do Azure](/azure/storage/storage-create-storage-account).                                                                                                                                                                                                              |
| Hyper-V – baixe o agente dos Serviços de Recuperação do Microsoft Azure e instale-o nos servidores do host Hyper-V.                                                                                                                                     |
| VMWare — verifique se o serviço de mobilidade está instalado em todas as VMs.                                                                                                                                                                           |
| [Habilite a proteção para VMs na nuvem do VMM, sites de Hyper-V ou sites de VMWare](rds-enable-dr-with-asr.md).                                                                                                                                                                    |
| **Etapa 3 – Criar seu plano de recuperação.**                                                                                                                                                                                                        |
| Mapeie seus recursos – mapeie as redes locais para as VNETs do Azure.                                                                                                                                                                              |
| [Crie o plano de recuperação](rds-disaster-recovery-plan.md). |
| Teste o plano de recuperação criando um failover de teste. Certifique-se de que todas as VMs possam acessar os recursos necessários, como o Active Directory. Verifique se os redirecionamentos de rede estão configurados e funcionando para o RDS. Para obter etapas detalhadas sobre como testar seu plano de recuperação, confira [Executar um failover de teste](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **Etapa 4 – Executar uma simulação de recuperação de desastre.**                                                                                                                                                                                                     |
| Execute uma simulação de recuperação de desastres usando failovers planejados e não planejados. Certifique-se de que todas as máquinas virtuais tenham acesso aos recursos necessários, como o Active Directory. Certifique-se de que todas as máquinas virtuais tenham acesso aos recursos necessários, como o Active Directory. Para ver as etapas detalhadas dos failovers e como executar simulações, confira [Failover no Site Recovery](/azure/site-recovery/site-recovery-failover).|


