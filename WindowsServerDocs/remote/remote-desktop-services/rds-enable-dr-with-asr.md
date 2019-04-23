---
title: Habilitar a recuperação de desastres do RDS usando o Azure Site Recovery
description: Saiba como habilitar a recuperação de desastres do RDS usando o Azure Site Recovery.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 05/05/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
manager: dongill
ms.openlocfilehash: e3f9db4afb37452b4fd5d0229b385492b915fe45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59859007"
---
# <a name="enable-disaster-recovery-of-rds-using-azure-site-recovery"></a>Habilitar a recuperação de desastres do RDS usando o Azure Site Recovery

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Para garantir que sua implantação do RDS está adequadamente configurada para recuperação de desastres, você precisa proteger todos os componentes que compõem sua implantação do RDS:

- Active Directory
- SQL Server tier
- Componentes RDS
- Componentes de rede
 
## <a name="configure-active-directory-and-dns-replication"></a>Configurar a replicação do Active Directory e DNS

Você precisa do Active Directory no site de recuperação de desastre para sua implantação do RDS para trabalhar. Você tem duas opções com base em sua implantação do RDS quão complexo é:

- Opção 1 – se você tiver um número pequeno de aplicativos e um único controlador de domínio para todo o seu site local e você será estar falhando ao longo de todo o site conjuntamente, use a replicação do ASR para replicar o controlador de domínio para o site secundário (verdadeiro para ambos cenários site a site e site para o Azure).
- Opção 2: se você tiver um grande número de aplicativos e você estiver executando uma floresta do Active Directory e você vai failover de alguns aplicativos por vez, configure um controlador de domínio adicional no site de recuperação de desastres (em um site secundário ou no Azure).

Ver [proteger o Active Directory e DNS com o Azure Site Recovery](/azure/site-recovery/site-recovery-active-directory) para obter detalhes sobre como disponibilizar um controlador de domínio no site de recuperação de desastre. Para o restante deste guia, presumimos que você seguiu essas etapas e tem o controlador de domínio disponível.

## <a name="set-up-sql-server-replication"></a>Configurar a replicação do SQL Server

Ver [proteger o SQL Server usando a recuperação de desastres do SQL Server e o Azure Site Recovery](/azure/site-recovery/site-recovery-sql) para obter as etapas configurar a replicação do SQL Server.

## <a name="enable-protection-for-the-rds-application-components"></a>Habilitar a proteção para os componentes de aplicativo do RDS

Dependendo do seu tipo de implantação do RDS, você pode habilitar proteção para VMs de componente diferentes (conforme listado na tabela a seguir) no Azure Site Recovery. Configure os elementos relevantes do Azure Site Recovery com base em se suas VMs são implantadas no Hyper-V ou VMWare.

| Tipo de implantação                              | Etapas de proteção                                                                                                                                                                                      |
|----------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Área de trabalho virtual (não gerenciada)         |  1. Verifique se que todos os hosts de virtualização estão prontos com a função RDVH instalada.    </br>2. Agente de Conexão.  </br>3. Áreas de trabalho pessoais. </br>4. VM do modelo Gold. </br>5. Web Access, servidor de licença e o servidor de Gateway |
| Com o pool de área de trabalho virtual (gerenciada com nenhuma UPD) |  1. Todos os hosts de virtualização estão prontos com a função RDVH instalada.  </br>2. Agente de Conexão.  </br>3. VM do modelo Gold. </br>4. Web Access, servidor de licença e o servidor de Gateway.                                  |
| Aplicativos remotos e sessões da área de trabalho (sem UDP)     |  1. Hosts de sessão.  </br>2. Agente de Conexão. </br>3. Web Access, servidor de licença e o servidor de Gateway.                                                                                                          |                                                                                                                                      |

