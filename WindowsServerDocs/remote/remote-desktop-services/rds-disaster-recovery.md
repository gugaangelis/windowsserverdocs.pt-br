---
title: Proteger sua implantação do RDS - recuperação de desastre
description: Saiba mais sobre as opções de recuperação de desastres para os serviços de área de trabalho remota
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9ff6a3b0-ea14-424e-9524-209252e9f1a8
author: lizap
ms.author: elizapo
ms.date: 06/12/2017
ms.openlocfilehash: a6eac3a50999633d15b1b6dc28608f60f6fef6c7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834077"
---
# <a name="configure-disaster-recovery-for-remote-desktop-services"></a>Configurar a recuperação de desastres para os serviços de área de trabalho remota

Quando você implanta serviços de área de trabalho remota em seu ambiente, ele se torna uma parte essencial de sua infra-estrutura, especialmente os aplicativos e recursos que você compartilhar com usuários. Se a implantação de RDS ficar inativo devido a qualquer coisa de uma falha de rede a um desastre natural, usuários não podem acessar esses aplicativos e recursos e seus negócios sejam afetados negativamente. Para evitar isso, você pode configurar uma solução de recuperação de desastres que permite que você failover sua implantação - se sua implantação do RDS estiver disponível, por algum motivo, há um backup disponível para assumir o controle automaticamente.

Para manter sua implantação do RDS em execução no caso de um único componente ou o computador ficar inativo, é recomendável configurar a implantação de RDS para alta disponibilidade. Você pode fazer isso configurando um [farm RDSH](rds-scale-rdsh-farm.md) e garantir que seu [agentes de Conexão estão clusterizados para alta disponibilidade](rds-connection-broker-cluster.md). 

As soluções de recuperação de desastres que recomendamos aqui são proteger sua implantação de um desastre grave - algo que desativa a implantação de RDS inteira (incluindo funções redundantes configuradas para alta disponibilidade). Se tal desastre atinge, ter uma solução de recuperação de desastres incorporada a sua implantação permitem que você failover toda a implantação e obter rapidamente aplicativos e recursos de backup e em execução para seus usuários.

Use as informações a seguir para implantar soluções de recuperação de desastres em RDS:

- [Aproveitar vários data centers do Azure para garantir que os usuários podem acessar a sua implantação do RDS, mesmo se um data center do Azure fica inativo (redundância geográfica)](rds-multi-datacenter-deployment.md)
- [Implantar o Azure Site Recovery para fornecer um failover para componentes RDS em failovers de site a site ou site para o Azure](rds-disaster-recovery-with-azure.md)


