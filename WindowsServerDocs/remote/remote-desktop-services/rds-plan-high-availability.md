---
title: Serviços de área de trabalho remota - alta disponibilidade
description: Planejamento de informações sobre como configurar uma implantação de RDS altamente disponível.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ec630ea0-ab80-4dfe-a25f-f4f601651f72
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: b5a2bd38c8831063d6fd2ba525b71a10403b8fc2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839257"
---
# <a name="remote-desktop-services---high-availability"></a>Serviços de área de trabalho remota - alta disponibilidade

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Falhas e limitação são inevitáveis em sistemas de grande porte. É simple configurar funções de infraestrutura de área de trabalho remota para dar suporte à alta disponibilidade e permitir que os usuários finais para conectar-se perfeitamente, toda vez.

Nos serviços de área de trabalho remota, os itens a seguir representam as funções de infraestrutura de área de trabalho remota, com suas respectivas diretrizes para estabelecer a alta disponibilidade:
- [Agente de Conexão de área de trabalho remota](Deploy-a-Remote-Desktop-Connection-Broker-cluster.md)
- [Gateway de área de trabalho remota](Deploy-a-RD-Web-Access-and-Gateway-farm.md)
- Licenciamento de Área de Trabalho Remota
- [Acesso via Web da área de trabalho remota](Deploy-a-RD-Web-Access-and-Gateway-farm.md)

Alta disponibilidade é estabelecida duplicando cada um dos serviços de funções em um segundo máquinas. No Azure, você pode receber um tempo de atividade garantido, colocando o conjunto de duas máquinas virtuais (hospedando a mesma função) em um disponibilidade define.

Juntamente com conjuntos de disponibilidade, agora você pode aproveitar o poder do banco de dados SQL do Azure e seu SLA com suporte do Azure para garantir que você sempre tenha as informações de conexão e pode redirecionar usuários para seus desktops e aplicativos.

Para práticas recomendadas sobre a criação de seu ambiente de RDS, consulte o [arquitetura de hospedagem de área de trabalho](desktop-hosting-reference-architecture.md).