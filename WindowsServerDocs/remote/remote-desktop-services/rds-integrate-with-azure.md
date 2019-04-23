---
title: RDS - integração com serviços do Azure
description: Saiba como integrar o RDS em sua implantação do Azure e o Azure em sua implantação do RDS.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/18/2017
ms.tgt_pltfrm: na
ms.topic: article
author: lizap
ms.openlocfilehash: e582612496591356ed96b34522333d0e8bf34093
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879597"
---
# <a name="remote-desktop-services---integrating-with-azure-services"></a>Serviços de área de trabalho remota - a integração com serviços do Azure

Windows Server 2016 combina a entrega segura eficiente de desktops e aplicativos por meio de serviços de área de trabalho remota com os serviços de flexíveis e escalonáveis fornecidos pelo Microsoft Azure. Você pode implantar RDS com serviços do Azure para ajudar a reduzir os custos de manutenção de infraestrutura para servidores locais, aumentar a estabilidade, usando os serviços do Azure para garantir a alta disponibilidade, melhorar a segurança usando a autenticação multifator e melhorar seu experiência dos usuários usando identidades existentes para acessar recursos no RDS.

Use as informações a seguir para integrar o Azure em sua implantação de área de trabalho remota:

- [Saiba como usar a autenticação multifator com o RDS](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway)
- [Integrar o Azure AD Domain Services com a implantação do RDS](rds-azure-adds.md)
- [Publicar a área de trabalho remota com o Proxy de aplicativo do Azure AD](/azure/active-directory/application-proxy-publish-remote-desktop)

Para ver como esses serviços simplificam a arquitetura da sua implantação de área de trabalho remota, fazer check-out [arquiteturas RDS com funções de PaaS do Azure exclusivas](desktop-hosting-logical-architecture.md#rds-architectures-with-unique-azure-paas-roles).