---
title: Serviços de Área de Trabalho Remota – Autenticação Multifator
description: Informações de planejamento para usar a MFA com RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 179feca4870e62f81ed71fabb7b8fd1cb418d391
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86962178"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>Serviços de Área de Trabalho Remota – Autenticação Multifator

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Aproveite o poder do Active Directory com a Autenticação Multifator para aplicar a proteção de alta segurança a seus recursos de negócios.

Para os usuários finais que se conectam a suas áreas de trabalho e aplicativos, a experiência é semelhante à que eles já têm, pois eles realizam uma segunda etapa de autenticação para se conectarem ao recurso desejado:
- Iniciar uma área de trabalho ou RemoteApp de um arquivo RDP ou por meio de um aplicativo de cliente de área de trabalho remota
- Após a conexão com o gateway de área de trabalho remota para acesso remoto seguro, receba um SMS ou um desafio de MFA no aplicativo móvel
- Autentique corretamente e conecte-se aos seus recursos.

Para obter mais detalhes sobre o processo de configuração, confira [Integrar a infraestrutura do gateway de área de trabalho remota usando a extensão de NPS (Servidor de Políticas de Rede) e o Azure AD](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway).
