---
title: Extensões para o Windows Admin Center
description: Extensões para o SDK do Windows Admin Center (Project Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fa3d7e75b32f0195346e58db54b7932c8d2fd3b9
ms.sourcegitcommit: 659544db1e19d6eecc52c7de07116ae735280544
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/11/2019
ms.locfileid: "9001838"
---
# Extensões para o Windows Admin Center

>Aplicável a: Windows Admin Center, Windows Admin Center Preview

O Windows Admin Center foi criado como uma plataforma extensível para permitir aos parceiros e desenvolvedores aproveitar as funcionalidades existentes no Windows Admin Center, integrar com outros produtos de administração de TI e soluções, e fornecer valor adicional para clientes. Cada ferramenta e solução no Windows Admin Center é criada como uma extensão usando os mesmos recursos de extensibilidade disponíveis para parceiros e desenvolvedores. Dessa forma, você poder criar ferramentas tão eficientes como as que estão disponíveis no Windows Admin Center hoje.

As extensões do Windows Admin Center são criadas usando tecnologias da Web modernas, incluindo o HTML5, CSS, Angular, TypeScript e jQuery, e podem gerenciar os servidores de destino pelo PowerShell ou WMI. Você também pode gerenciar servidores de destino, serviços ou dispositivos através de diferentes protocolos, como o REST ao aproveitar um plug-in de gateway do Windows Admin Center.

## Por que você deve considerar a desenvolver uma extensão para o Windows Admin Center

Entenda o valor que você pode trazer para seu produto e os clientes com o desenvolvimento de extensões para o Windows Admin Center:

- **Integração com as ferramentas do Windows Admin Center:** integre seus produtos e serviços com ferramentas de gerenciamento de servidor e cluster no Windows Admin Center, além de oferecer monitoramento, gerenciamento e experiências de solução de problemas unificadas, em interrupção e de ponta a ponta.
- **Aproveitar os recursos de segurança, identidade e gerenciamento de plataforma:** habilite o suporte do Azure Active Directory (AAD), a autenticação multifator, o controle de acesso baseado em função (RBAC), o registro em log, a auditoria para seu produto e serviços, aproveitando recursos de plataforma do Windows Admin Center para atender aos requisitos complexos das organizações de TI atuais.
- **Desenvolver usando as tecnologias mais recentes da Web:** crie rapidamente experiências do usuário impressionantes usando tecnologias de Web moderna, incluindo o HTML5, CSS, angulares, TypeScript e jQuery e controles de interface de usuário avançados incluídos no SDK do Windows Admin Center.
- **Estender o alcance do produto:** faça parte do ecossistema novo do Windows Admin Center com alcance rápido para nossa base de clientes em expansão e aproveite o lançamento do Windows Server 2019 posteriormente neste ano.

## Começar a desenvolver com o SDK do Windows Admin Center

Introdução ao desenvolvimento do Windows Admin Center é fácil!  Exemplo de código pode ser encontrado para tipos de extensão de [ferramenta](develop-tool.md), [soluções](develop-solution.md)e [plug-in de gateway](develop-gateway-plugin.md) em nossa documentação do SDK. Lá, você aproveitará a CLI do Windows Admin Center para criar um novo projeto de extensão e siga os guias individuais para personalizar seu projeto para atender às suas necessidades.

Fizemos um Windows Admin Center [SDK Kit de ferramentas de design](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) disponíveis para ajudá-lo rapidamente simular extensões no PowerPoint usando estilos do Windows Admin Center, controles e modelos de página. Consulte sua extensão possível aparência no Centro de administração do Windows antes de começar a codificação!

Também temos o código de exemplo hospedado no GitHub: [Ferramentas de desenvolvedor](https://aka.ms/wacsdk) é uma extensão de solução de exemplo que contém um conjunto rico de controles que você pode procurar e usar em sua própria extensão. Ferramentas de desenvolvedor é uma extensão totalmente funcional que pode ser carregada no Windows Admin Center no modo do desenvolvedor.

Consulte os tópicos a seguir para saber mais sobre o SDK e começar:

- [Compreender como extensões funcionam](understand-extensions.md)
- [Desenvolver uma extensão](developing-extensions.md)
- [Guias](guides.md)
- [Publicar sua extensão](publish-extensions.md)

## Destaque de parceiro

Veja o valor incrível que nossos parceiros começaram a trazer para o ecossistema do Windows Admin Center e teste essas extensões hoje mesmo. Saiba mais sobre [como instalar as extensões](../configure/using-extensions.md) do Windows Admin Center.

### DataON

Extensão DataON MUST traz monitoramento, gerenciamento e ponta a ponta insight DataON hiperconvergente infraestrutura e sistemas de armazenamento com base no Windows Server. A extensão deve adiciona um valor exclusivo, como relatórios de dados históricos, mapeamento de disco, alertas do sistema e serviço call de SAN semelhantes home, complementando o servidor do Windows Admin Center e os recursos de gerenciamento da infraestrutura hiperconvergente, por meio de uma perfeita, experiência unificada. [Saiba mais sobre a extensão DataON MUST e sua experiência de desenvolvimento](case-studies/dataon.md).

![Extensão DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### Fujitsu

As extensões ServerView Health e RAID Health da Fujitsu para Windows Admin Center fornecem monitoramento e gerenciamento aprofundado de componentes de hardware críticos como processadores, memória, energia e subsistemas de armazenamento para servidores PRIMERGY da Fujitsu. Ao usar os padrões de design UX do Windows Admin Center e controles da interface do usuário, a Fujitsu nos ajudou a realizar a visão de informações de ponta a ponta de funções e serviços do servidor, para sistema operacional e gerenciamento de hardware por meio da plataforma do Windows Admin Center. [Saiba mais sobre as extensões da Fujitsu e sua experiência de desenvolvimento](case-studies/fujitsu.md).

![Extensão ServerView da Fujitsu](../media/extensibility-overview/fujitsu-serverview-extension.png)

### Lenovo

Extensão de integrador de XClarity do Lenovo leva o gerenciamento de hardware para o próximo nível integrando-se perfeitamente em várias experiências no Windows Admin Center. A solução XClarity integrador fornece uma visão geral de todos os seus servidores Lenovo e extensões de ferramenta diferentes fornecem detalhes de hardware, se você estiver conectado a um único servidor, cluster de failover ou um cluster hiperconvergente. [Saiba mais sobre a extensão do Lenovo XClarity integrador](case-studies/lenovo.md).

![Extensão da Lenovo](../media/extensibility-overview/lenovo-extension.png)

### Armazenamento puro

Armazenamento puro fornece enterprise, soluções de armazenamento de dados de todos os flash que oferecem arquitetura centrados em dados para acelerar sua empresa para uma vantagem competitiva. A extensão de armazenamento pura para o Windows Admin Center fornece uma exibição de painel único em produtos FlashArray pura e permite que os usuários a realizar tarefas de monitoramento, exibir métricas de desempenho em tempo real e gerenciar volumes de armazenamento e iniciadores por meio de uma única interface do usuário experiência. [Saiba mais sobre as extensões do puro e sua experiência de desenvolvimento](case-studies/purestorage.md).

![Extensão de armazenamento pura](../media/extensibility-overview/purestorage-extension.png)

### Squared Up

A Squared Up fornece a melhor experiência de monitoramento da classe com base no System Center Operations Manager e integração com o Azure Log Analytics, o Application Insights e outras soluções de monitoramento. A [extensão quadradas backup](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) traz dados históricos de desempenho e topologias de aplicativos em tempo real e dependências para o contexto do servidor e o gerenciamento de cluster que fornece o Windows Admin Center e clientes iniciais tem aclamados o valor da trazendo dados enorme de muitas fontes diferentes em uma única experiência. [Saiba mais sobre a extensão da Squared Up e sua experiência de desenvolvimento](case-studies/squared-up.md).

![Extensão da Squared Up](../media/extensibility-overview/squaredup-extension.png)