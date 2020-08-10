---
title: Serviços de Área de Trabalho Remota – Autenticação Multifator
description: Informações de planejamento para usar a MFA com RDS.
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 8d1edf1db673c70895e228a059bef753c617f617
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954963"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>Serviços de Área de Trabalho Remota – Autenticação Multifator

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Aproveite o poder do Active Directory com a Autenticação Multifator para aplicar a proteção de alta segurança a seus recursos de negócios.

Para os usuários finais que se conectam a suas áreas de trabalho e aplicativos, a experiência é semelhante à que eles já têm, pois eles realizam uma segunda etapa de autenticação para se conectarem ao recurso desejado:
- Iniciar uma área de trabalho ou RemoteApp de um arquivo RDP ou por meio de um aplicativo de cliente de área de trabalho remota
- Após a conexão com o gateway de área de trabalho remota para acesso remoto seguro, receba um SMS ou um desafio de MFA no aplicativo móvel
- Autentique corretamente e conecte-se aos seus recursos.

Para obter mais detalhes sobre o processo de configuração, confira [Integrar a infraestrutura do gateway de área de trabalho remota usando a extensão de NPS (Servidor de Políticas de Rede) e o Azure AD](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway).
