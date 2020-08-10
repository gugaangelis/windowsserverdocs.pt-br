---
title: Serviços de Área de Trabalho Remota – Alta disponibilidade
description: Informações de planejamento sobre como configurar uma implantação de RDS altamente disponível.
ms.topic: article
ms.assetid: ec630ea0-ab80-4dfe-a25f-f4f601651f72
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: c50c66bee8f9f387497ef2f5971dbee5e1b402c6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955003"
---
# <a name="remote-desktop-services---high-availability"></a>Serviços de Área de Trabalho Remota – Alta disponibilidade

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Falhas e limitação são inevitáveis em sistemas de grande porte. É simples configurar funções de infraestrutura de Área de Trabalho Remota para dar suporte à alta disponibilidade e permitir que os usuários finais se conectem toda vez com perfeição.

Nos Serviços de Área de Trabalho Remota, os itens a seguir representam as funções de infraestrutura de área de trabalho remota, com suas respectivas diretrizes para estabelecer a alta disponibilidade:
- [Agente de Conexão de Área de Trabalho Remota](./rds-connection-broker-cluster.md)
- [Gateway de Área de Trabalho Remota](./rds-rdweb-gateway-ha.md)
- Licenciamento de Área de Trabalho Remota
- [Acesso via Web à Área de Trabalho Remota](./rds-rdweb-gateway-ha.md)

A alta disponibilidade é estabelecida duplicando cada um dos serviços de funções em máquinas secundárias. No Azure, você pode receber um tempo de atividade garantido colocando o conjunto de duas máquinas virtuais (hospedando a mesma função) em conjuntos de disponibilidade.

Com os conjuntos de disponibilidade, agora é possível aproveitar o poder do banco de dados SQL do Azure e seu SLA com suporte do Azure para garantir que você sempre tenha as informações de conexão e possa redirecionar usuários para suas áreas de trabalho remotas e aplicativos.

Para acessar as práticas recomendadas sobre a criação de seu ambiente de RDS, confira a [arquitetura de hospedagem da área de trabalho](desktop-hosting-reference-architecture.md).
