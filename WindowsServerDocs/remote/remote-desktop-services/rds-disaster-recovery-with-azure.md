---
title: Configurar a recuperação de desastre para RDS usando a recuperação de desastre do Azure
description: Saiba como usar a recuperação de desastre do Azure para recuperação de desastre para uma implantação do RDS
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 06/12/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 561a515e23d12cc3397c40fd885550e735ed4d27
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878167"
---
# <a name="set-up-disaster-recovery-for-rds-using-azure-site-recovery"></a>Configurar a recuperação de desastre para RDS usando o Azure Site Recovery

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar o Azure Site Recovery para criar uma solução de recuperação de desastre para sua implantação de serviços de área de trabalho remota. 

[O Azure Site Recovery](/azure/site-recovery/site-recovery-overview) é um serviço baseado no Azure que fornece recursos de recuperação de desastre orquestrando a replicação, failover e recuperação de máquinas virtuais. O Azure Site Recovery dá suporte a um número de tecnologias de replicação para replicar de forma consistente, proteger e perfeitamente failover de máquinas virtuais e aplicativos privada/pública ou em nuvens do hoster. 

Use as informações a seguir para criar e validar a solução de recuperação de desastres.

## <a name="disaster-recovery-deployment-options"></a>Opções de implantação de recuperação de desastre

Você pode implantar RDS em servidores físicos ou máquinas virtuais que executam o Hyper-V ou VMWare. O Azure Site Recovery pode proteger fontes locais e implantações de virtual em um site secundário ou no Azure. A tabela a seguir mostra os diferentes suporte para implantações de RDS em cenários de recvoery de desastres de site a site e site para o Azure.

| Tipo de implantação                          | Site a site do Hyper-V | Site de Hyper-V e Azure | Site de VMWare e Azure | Físico site para o Azure |
|------------------------------------------|----------------------|-----------------------|---------------------|----------------------|-----------------------|------------------------|
| Em pool área de trabalho virtual (não gerenciada)       |Sim|Não|Não|Não |
| Área de trabalho virtual em pool (gerenciado, sem UPD) | Sim|Não|Não|Não|
| Sessões de RemoteApps e área de trabalho (sem UDP) | Sim|Sim|Sim|Sim  |

## <a name="prerequisites"></a>Pré-requisitos

Antes de configurar o Azure Site Recovery para sua implantação, certifique-se de que cumprir os seguintes requisitos:

- Criar uma [implantação de RDS locais](rds-deploy-infrastructure.md).
- Adicione [do Cofre de serviços do Azure Site Recovery](/azure/site-recovery/site-recovery-vmm-to-azure#create-a-recovery-services-vault) à sua assinatura do Microsoft Azure.
- Se você pretende usar o Azure como seu site de recuperação, execute as [ferramenta de avaliação de prontidão de máquina Virtual do Azure](https://azure.microsoft.com/downloads/vm-readiness-assessment/) em suas VMs para garantir que eles sejam compatíveis com VMs do Azure e serviços do Azure Site Recovery.
 
## <a name="implementation-checklist"></a>Lista de verificação de implementação

Abordaremos as várias etapas para habilitar serviços de recuperação de Site do Azure para sua implantação do RDS em mais detalhes, mas aqui estão as etapas de implementação de alto nível.

| **Etapa 1 – Configurar VMs para recuperação de desastres**                                                                                                                                                                                               |
|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Hyper-V - baixar o provedor do Microsoft Azure Site Recovery. Para instalá-lo em seu servidor do VMM ou host Hyper-V. Ver [pré-requisitos para replicação no Azure usando o Azure Site Recovery](/azure/site-recovery/site-recovery-prereq) para obter informações.                                                                                                                             |
| VMWare — configurar servidor de proteção, servidor de configuração e servidores de destino mestre                                                                                                                                                      |
| **Etapa 2: preparar os seus recursos**                                                                                                                                                                                                           |
| Adicionar um [conta de armazenamento do Azure](/azure/storage/storage-create-storage-account).                                                                                                                                                                                                              |
| Hyper-V - Baixe o agente de serviços de recuperação do Microsoft Azure e instalá-lo em servidores host Hyper-V.                                                                                                                                     |
| VMWare — Verifique se o serviço de mobilidade está instalado em todas as VMs.                                                                                                                                                                           |
| [Habilitar a proteção para VMs na nuvem do VMM, sites do Hyper-V ou VMWare locais](rds-enable-dr-with-asr.md).                                                                                                                                                                    |
| **Etapa 3: criar seu plano de recuperação.**                                                                                                                                                                                                        |
| Mapear seus recursos - mapa redes locais para redes virtuais do Azure.                                                                                                                                                                              |
| [Criar o plano de recuperação](rds-disaster-recovery-plan.md). |
| Teste o plano de recuperação com a criação de um failover de teste. Certifique-se de que todas as VMs podem acessar os recursos necessários, como o Active Directory. Verifique se a rede redirecionamentos são configurados e funcionando para RDS. Para obter etapas detalhadas sobre como testar seu plano de recuperação, consulte [executar um failover de teste](/azure/site-recovery/site-recovery-test-failover-to-azure)|
| **Etapa 4: executar uma simulação de recuperação de desastres.**                                                                                                                                                                                                     |
| Execute uma simulação de recuperação de desastres usando failovers planejados e não planejado. Certifique-se de que todas as máquinas virtuais tenham acesso aos recursos necessários, como o Active Directory. Certifique-se de que todas as máquinas virtuais tenham acesso aos recursos necessários, como o Active Directory. Para obter etapas detalhadas sobre failovers e como executar os exercícios, consulte [Failover no Site Recovery](/azure/site-recovery/site-recovery-failover).|


