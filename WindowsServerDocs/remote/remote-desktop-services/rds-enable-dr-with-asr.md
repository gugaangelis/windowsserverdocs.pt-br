---
title: Habilitar a recuperação de desastre do RDS usando o Azure Site Recovery
description: Saiba como habilitar a recuperação de desastre do RDS usando o Azure Site Recovery.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: 0c7af18be4aa767009f1dd0b82f145ffe6874768
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80861399"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>Habilitar a recuperação de desastre do RDS usando o Azure Site Recovery

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Para garantir que sua implantação do RDS esteja configurada adequadamente para a recuperação de desastres, você precisa proteger todos os componentes que compõem sua implantação do RDS:

- Active Directory
- Nível do SQL Server
- Componentes do RDS
- Componentes de rede

## <a name="configure-active-directory-and-dns-replication"></a>Configurar o Active Directory e a replicação DNS

Você precisa do Active Directory no site de recuperação de desastre para que sua implantação do RDS funcione. Você tem duas opções com base na complexidade da implantação de RDS:

- Opção 1 – se você tiver um número pequeno de aplicativos e um único controlador de domínio para todo seu site local e quiser fazer o failover de todo o site em conjunto, use a replicação de ASR para replicar o controlador de domínio para o site secundário (válido para cenários site a site e site ao Azure).
- Opção 2 – se você tiver um grande número de aplicativos, estiver executando uma floresta do Active Directory e quiser fazer failover de alguns aplicativos por vez, configure um controlador de domínio adicional no site de recuperação de desastre (um site secundário ou no Azure).

Consulte [Proteger o Active Directory e o DNS com o Azure Site Recovery](/azure/site-recovery/site-recovery-active-directory) para obter detalhes sobre como disponibilizar um controlador de domínio no site de recuperação de desastre. Para o restante deste guia, presumimos que você seguiu essas etapas e tem o controlador de domínio disponível.

## <a name="set-up-sql-server-replication"></a>Configurar a Replicação do SQL Server

Consulte [Proteger o SQL Server usando a recuperação de desastre do SQL Server e o Azure Site Recovery](/azure/site-recovery/site-recovery-sql) para obter as etapas para configurar a Replicação do SQL Server.

## <a name="enable-protection-for-the-rds-application-components"></a>Habilitar a proteção para componentes de aplicativo do RDS

Dependendo do tipo de implantação do RDS, você pode habilitar a proteção para VMs de componentes diferentes (conforme listado na tabela a seguir) no Azure Site Recovery. Configure os elementos relevantes do Azure Site Recovery com base em suas VMs estarem implantadas no Hyper-V ou no VMWare.


|               Tipo de implantação                |                                                                                                     Etapas de proteção                                                                                                     |
|----------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|     Área de trabalho virtual pessoal (não gerenciada)     | 1. Verifique se todos os hosts de virtualização estão prontos com a função de RDVH instalada.    </br>2. Agente de Conexão.  </br>3. Áreas de trabalho pessoais. </br>4. VM modelo Gold. </br>5. Acesso via Web, servidor de licença e Servidor de gateway |
| Área de trabalho virtual em pool (gerenciada, sem UPD) |                    1. Todos os hosts de virtualização estão prontos com a função de RDVH instalada.  </br>2. Agente de Conexão.  </br>3. VM modelo Gold. </br>4. Acesso via Web, servidor de licença e servidor de gateway.                    |
|   Sessões de RemoteApps e área de trabalho (sem UPD)   |                                                          1. Hosts de sessão.  </br>2. Agente de Conexão. </br>3. Acesso via Web, servidor de licença e servidor de gateway.                                                           |

