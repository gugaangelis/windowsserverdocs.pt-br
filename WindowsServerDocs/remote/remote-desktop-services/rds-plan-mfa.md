---
title: Serviços de área de trabalho remota - a autenticação multifator
description: Informações de planejamento para usar o MFA com RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09ea784e-5644-417a-a3d9-bdbcebc313f9
author: lizap
ms.author: elizapo
ms.date: 09/07/2016
manager: dongill
ms.openlocfilehash: 50cbabf377e5b01c44360d776b9ff999826303c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814867"
---
# <a name="remote-desktop-services---multi-factor-authentication"></a>Serviços de área de trabalho remota - a autenticação multifator

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Aproveite o poder do Active Directory com a autenticação multifator para aplicar a proteção de alta segurança de seus recursos de negócios.

Para os usuários finais se conectar a suas áreas de trabalho e aplicativos, a experiência é semelhante a que eles já enfrentam conforme realizam uma segunda medida de autenticação para se conectar ao recurso desejado:
- Iniciar uma área de trabalho ou RemoteApp a partir de um arquivo RDP ou por meio de um aplicativo de cliente de área de trabalho remota
- Após a conexão com o Gateway de área de trabalho remota para acesso remoto seguro, receba um SMS ou aplicativo móvel desafio de MFA
- Corretamente autentique e conecte-se aos seus recursos!

Para obter mais detalhes sobre o processo de configuração, confira [integrar sua infraestrutura do Gateway de área de trabalho remota usando a extensão de servidor de diretivas de rede (NPS) e o Azure AD ](https://docs.microsoft.com/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway).
