---
title: RDS – Integração com os serviços do Azure
description: Saiba como integrar o RDS à sua implantação do Azure e o Azure à sua implantação do RDS.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: elizapo
ms.date: 08/18/2017
ms.topic: article
author: lizap
ms.openlocfilehash: 2c792a22608fe0e7ae826b27d6917202037559e4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855069"
---
# <a name="remote-desktop-services---integrating-with-azure-services"></a>Serviços de Área de Trabalho Remota – Integração com os serviços do Azure

O Windows Server 2016 combina a entrega segura e avançada de áreas de trabalho e aplicativos por meio dos Serviços de Área de Trabalho Remota com os serviços de flexíveis e dimensionáveis fornecidos pelo Microsoft Azure. Você pode implantar o RDS com serviços do Azure para ajudar a reduzir os custos de manutenção de infraestrutura para servidores locais, aumentar a estabilidade usando os serviços do Azure para garantir alta disponibilidade, aumentar a segurança usando a autenticação multifator e melhorar a experiência dos usuários usando identidades existentes para acessar recursos no RDS.

Use as informações a seguir para integrar o Azure à sua implantação da Área de Trabalho Remota:

- [Saiba como usar a autenticação multifator com o RDS](/azure/multi-factor-authentication/nps-extension-remote-desktop-gateway)
- [Integrar o Azure AD Domain Services à implantação do RDS](rds-azure-adds.md)
- [Publicar a Área de Trabalho Remota com o Proxy de aplicativo do Azure AD](/azure/active-directory/application-proxy-publish-remote-desktop)

Para ver como esses serviços simplificam a arquitetura de sua implantação da Área de Trabalho Remota, confira [Arquiteturas de RDS com funções exclusivas de PaaS do Azure](desktop-hosting-logical-architecture.md#rds-architectures-with-unique-azure-paas-roles).