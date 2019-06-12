---
title: Serviços de área de trabalho bem-vindas ao remoto no Windows Server 2016
description: Fornece uma visão geral dos serviços de área de trabalho remota
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
ms.openlocfilehash: 3d148c99911be0cebfc29429d93241f24c2b9606
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66453011"
---
# <a name="welcome-to-remote-desktop-services"></a>Bem-vindo aos serviços de área de trabalho remota 

Serviços de área de trabalho remota (RDS) é a plataforma ideal para a criação de soluções de virtualização para cada necessidade do cliente final, incluindo o fornecimento de aplicativos virtualizados individuais, fornecendo acesso seguro de área de trabalho móvel e remoto e fornecer aos usuários finais a capacidade de executar seus aplicativos e áreas de trabalho da nuvem.

![Visão geral de serviços de área de trabalho remota](./media/rds-overview.png)

RDS oferece a flexibilidade de implantação, extensibilidade e eficiência de custo — tudo entregue por meio de uma variedade de opções de implantação, incluindo o Windows Server 2016 para implantações locais, Microsoft Azure para implantações de nuvem e uma matriz robusta de parceiro soluções.

Dependendo do seu ambiente e preferências, você pode configurar a solução RDS para virtualização baseada em sessão, como uma virtual desktop infrastructure (VDI), ou uma combinação dos dois:

- **Virtualização baseada em sessão**: Aproveite o poder de computação do Windows Server para fornecer um ambiente de várias sessões e econômico para cargas de trabalho cotidiano dos usuários da unidade
- **VDI**: Aproveite o cliente do Windows para fornecer o alto desempenho, a compatibilidade de aplicativos e a familiaridade que seus usuários já conhecem de sua experiência de área de trabalho do Windows.

Dentro desses ambientes de virtualização, você tem flexibilidade adicional em Publicar para seus usuários:

- **Áreas de trabalho**: Dar aos usuários uma experiência de área de trabalho completa com uma variedade de aplicativos que você instalar e gerenciar. Ideal para os usuários que se baseiam nesses computadores como suas estações de trabalho primárias ou que são provenientes de clientes finos, como com o MultiPoint Services.
- **RemoteApps**: Especificar aplicativos individuais que são hospedados/execução no computador virtualizado, mas aparecem como se estiver em execução na área de trabalho do usuário, como aplicativos locais. Os aplicativos têm suas próprias entradas na barra de tarefas e podem ser redimensionados e movidos em monitores. Ideal para implantar e gerenciar aplicativos-chave no ambiente seguro, remoto, permitindo aos usuários trabalhar a partir e personalizar suas próprias áreas de trabalho.

Para ambientes em que o custo-benefício é crucial e você deseja estender os benefícios da implantação de áreas de trabalho completas em um ambiente de virtualização baseada em sessão, você pode usar [MultiPoint Services](../multipoint-services/multipoint-services.md) para entregar o melhor valor. 

Com essas opções e configurações, você tem a flexibilidade de implantação de desktops e aplicativos que os usuários precisam em um remoto, seguro e uma maneira econômica.

## <a name="next-steps"></a>Próximas etapas

Aqui estão algumas das próximas etapas para ajudá-lo a obter uma melhor compreensão do RDS e até mesmo começar a implantar seu próprio ambiente:
-   Entender os [configurações com suporte](rds-supported-config.md) RDS com as versões de vários Windows e Windows Server
-   [Planejar e projetar](rds-plan-and-design.md) um ambiente de RDS para acomodar vários requisitos, como alta disponibilidade e a autenticação multifator.
-   Examine os [modelos de arquitetura de serviços de área de trabalho remota](desktop-hosting-logical-architecture.md) que funcionam melhor para seu ambiente desejado.
-   Começar a [implantar seu ambiente de RDS com o ARM e o Azure Marketplace](rds-in-azure.md).
