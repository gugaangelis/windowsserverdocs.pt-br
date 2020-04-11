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
ms.openlocfilehash: c46ad24c62510b4a100a89b5c10a8f52c1a66151
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857349"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>Serviços de Área de Trabalho Remota – Autenticação Multifator

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016

Aproveite o poder do Active Directory com a Autenticação Multifator para aplicar a proteção de alta segurança a seus recursos de negócios.

Para os usuários finais que se conectam a suas áreas de trabalho e aplicativos, a experiência é semelhante à que eles já têm, pois eles realizam uma segunda etapa de autenticação para se conectarem ao recurso desejado:
- Iniciar uma área de trabalho ou RemoteApp de um arquivo RDP ou por meio de um aplicativo de cliente de área de trabalho remota
- Após a conexão com o gateway de área de trabalho remota para acesso remoto seguro, receba um SMS ou um desafio de MFA no aplicativo móvel
- Autentique corretamente e conecte-se aos seus recursos.

Para obter mais detalhes sobre o processo de configuração, confira [Integrar a infraestrutura do gateway de área de trabalho remota usando a extensão de NPS (Servidor de Políticas de Rede) e o Azure AD](https://docs.microsoft.com/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway).
