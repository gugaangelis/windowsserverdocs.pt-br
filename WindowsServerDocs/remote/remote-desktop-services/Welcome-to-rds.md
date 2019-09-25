---
title: Bem-vindo aos Serviços de Área de Trabalho Remota no Windows Server 2016
description: Fornece uma visão geral dos Serviços de Área de Trabalho Remota
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: chrimo
ms.date: 02/22/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 52b9e09f-39e0-41a9-9d3b-4d5f4eacf3e0
author: christianmontoya
manager: scottman
ms.localizationpriority: medium
ms.openlocfilehash: 1fcb3fb7e2989399d908a1b5bed7bd21240efeab
ms.sourcegitcommit: 2ec5e779c00481b13f186e2c56d207007897cfd4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/16/2019
ms.locfileid: "71021653"
---
# <a name="welcome-to-remote-desktop-services"></a>Bem-vindo aos serviços de área de trabalho remota 

Os RDS (Serviços de Área de Trabalho Remota) são a plataforma ideal para a criação de soluções de virtualização para cada necessidade do cliente final, incluindo fornecimento de aplicativos virtualizados individuais, de acesso seguro via Área de Trabalho Remota e dispositivos móveis e, por fim, da capacidade aos usuários finais de executarem seus aplicativos e áreas de trabalho da nuvem.

![Visão geral dos Serviços de Área de Trabalho Remota](./media/rds-overview.png)

O RDS oferece flexibilidade de implantação, extensibilidade e economia – tudo entregue por meio de uma variedade de opções de implantação, incluindo o Windows Server 2016 para implantações locais, o Microsoft Azure para implantações de nuvem e uma gama robusta de soluções de parceiro.

Dependendo do seu ambiente e preferências, você pode configurar a solução RDS para virtualização baseada em sessão, como uma VDI (infraestrutura de área de trabalho virtual) ou uma combinação dos dois:

- **Virtualização baseada em sessão**: aproveite o poder de computação do Windows Server para fornecer um ambiente de várias sessões econômico para conduzir as cargas de trabalho cotidianas dos usuários.
- **VDI**: Aproveite o cliente do Windows para fornecer o alto desempenho, a compatibilidade de aplicativos e a familiaridade que seus usuários já passaram a esperar de sua experiência desktop do Windows.

Dentro desses ambientes de virtualização, você tem flexibilidade adicional quanto ao que publicar para seus usuários:

- **Áreas de trabalho**: Dê aos usuários uma experiência de área de trabalho completa com uma variedade de aplicativos que você instala e gerencia. Ideal para os usuários que se baseiam nesses computadores como suas estações de trabalho primárias ou que são provenientes de clientes finos, tais como os Serviços MultiPoint.
- **RemoteApps**: especifique aplicativos individuais hospedados/executados na máquina virtualizada, mas que são exibidos como se estivessem em execução na área de trabalho do usuário, como aplicativos locais. Os aplicativos têm suas próprias entradas na barra de tarefas e podem ser redimensionados e movidos entre monitores. Ideal para implantar e gerenciar aplicativos-chave no ambiente remoto seguro, permitindo aos usuários trabalhar de suas próprias áreas de trabalho e personalizá-las.

Para ambientes em que o custo-benefício é crucial e em que você deseja estender os benefícios da implantação de áreas de trabalho completas em um ambiente de virtualização baseada em sessão, você pode usar os [Serviços MultiPoint](../multipoint-services/multipoint-services.md) para entregar o melhor valor. 

Com essas opções e configurações, você tem a flexibilidade de implantar as áreas de trabalho e aplicativos de que seus usuários precisam de modo remoto, seguro e econômico.

## <a name="next-steps"></a>Próximas etapas

Aqui estão algumas das próximas etapas para ajudá-lo a obter uma melhor compreensão do RDS e até mesmo começar a implantar seu próprio ambiente:
-   Compreender as [configurações compatíveis](rds-supported-config.md) para o RDS com as diversas versões do Windows e do Windows Server
-   [Planejar e projetar](rds-plan-and-design.md) um ambiente de RDS para acomodar vários requisitos, como alta disponibilidade e autenticação multifator.
-   Examine os [modelos de arquitetura dos Serviços de Área de Trabalho Remota](desktop-hosting-logical-architecture.md) que funcionam melhor para seu ambiente desejado.
-   Comece a [implantar um ambiente de RDS com o ARM e o Azure Marketplace](rds-in-azure.md).
