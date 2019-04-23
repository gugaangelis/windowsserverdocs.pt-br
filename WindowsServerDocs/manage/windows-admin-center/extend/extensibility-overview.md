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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884997"
---
# <a name="extensions-for-windows-admin-center"></a>Extensões para o Windows Admin Center

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

O Windows Admin Center foi criado como uma plataforma extensível para permitir aos parceiros e desenvolvedores aproveitar as funcionalidades existentes no Windows Admin Center, integrar com outros produtos de administração de TI e soluções, e fornecer valor adicional para clientes. Cada ferramenta e solução no Windows Admin Center é criada como uma extensão usando os mesmos recursos de extensibilidade disponíveis para parceiros e desenvolvedores. Dessa forma, você poder criar ferramentas tão eficientes como as que estão disponíveis no Windows Admin Center hoje.

As extensões do Windows Admin Center são criadas usando tecnologias da Web modernas, incluindo o HTML5, CSS, Angular, TypeScript e jQuery, e podem gerenciar os servidores de destino pelo PowerShell ou WMI. Você também pode gerenciar servidores de destino, serviços ou dispositivos através de diferentes protocolos, como o REST ao aproveitar um plug-in de gateway do Windows Admin Center.

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>Por que você deve considerar a desenvolver uma extensão para o Windows Admin Center

Entenda o valor que você pode trazer para seu produto e os clientes com o desenvolvimento de extensões para o Windows Admin Center:

- **Integre com ferramentas do Windows Admin Center:** Integrar seus produtos e serviços com ferramentas de gerenciamento de servidor e cluster no Windows Admin Center e entregar unificada e simples, a-ponta gerenciamento e monitoramento, solução de problemas de experiências para seus clientes.
- **Aproveite os recursos de segurança, identidade e gerenciamento da plataforma:** Habilitar suporte do Azure Active Directory (AAD), a autenticação multifator, controle de acesso baseado em função (RBAC), log e auditoria para seus produtos e serviços, aproveitando os recursos de plataforma do Windows Admin Center para atender aos requisitos complexos de hoje Organizações de TI.
- **Desenvolva usando as tecnologias da web mais recentes:** Crie rapidamente experiências de usuário impressionantes, uso de tecnologias de web modernos como HTML5, CSS, Angular, jQuery e TypeScript e controles de interface do usuário avançados e poderosos incluídos no SDK Windows Admin Center.
- **Estenda o alcance do produto:** Se tornam parte do ecossistema do Windows Admin Center novo com o atendimento a nossa base de clientes em crescimento rapidamente e aproveitar o Windows Server 2019 inicialização dinâmica posteriormente neste ano.

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>Comece a desenvolver com o SDK do Windows Admin Center

Introdução ao desenvolvimento do Windows Admin Center é fácil!  Código de exemplo pode ser encontrado para [ferramenta](develop-tool.md), [solução](develop-solution.md), e [plug-in do gateway](develop-gateway-plugin.md) tipos de extensão em nossa documentação do SDK. Lá, você utilizará a CLI do Windows Admin Center para criar um novo projeto de extensão e, em seguida, siga os guias individuais para personalizar seu projeto para atender às suas necessidades.

Fizemos uma do Windows Admin Center [Kit de ferramentas de design SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) disponíveis para ajudar você a rapidamente maquetes de extensões no PowerPoint usando estilos Windows Admin Center, controles e modelos de página. Veja sua extensão possível aparência em Windows Admin Center antes de começar a codificar!

Também temos o código de exemplo hospedado no GitHub: [Ferramentas de desenvolvedor](https://aka.ms/wacsdk) é uma extensão da solução de exemplo que contém um rico conjunto de controles que você pode procurar e usar em sua própria extensão. Ferramentas de desenvolvedor é uma extensão totalmente funcional que pode ser carregada no Windows Admin Center no modo do desenvolvedor.

Consulte os tópicos a seguir para saber mais sobre o SDK e começar:

- [Entender o funcionam de extensões](understand-extensions.md)
- [Desenvolver uma extensão](developing-extensions.md)
- [Guias](guides.md)
- [Publique sua extensão](publish-extensions.md)

## <a name="partner-spotlight"></a>Destaque de parceiro

Veja o valor incrível que nossos parceiros começaram a trazer para o ecossistema do Windows Admin Center e teste essas extensões hoje mesmo. Saiba mais sobre [como instalar as extensões](../configure/using-extensions.md) do Windows Admin Center.

### <a name="dataon"></a>DataON

Extensão do dados deve traz o monitoramento, gerenciamento e ponta a ponta insight de armazenamento e infraestrutura de sistemas hiperconvergentes do dados baseada no Windows Server. A extensão deve adiciona um valor exclusivo, como relatórios de dados históricos, mapeamento de disco, alertas do sistema e o serviço inicial chamada de SAN, complementando o servidor Windows Admin Center e recursos de gerenciamento de infraestrutura hiperconvergente, por meio de um contínuo experiência unificada. [Saiba mais sobre a extensão DataON MUST e sua experiência de desenvolvimento](case-studies/dataon.md).

![Extensão DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

ServerView integridade e a extensões de integridade de RAID para Windows Admin Center do Fujitsu oferecem profundo monitoramento e gerenciamento crítica de componentes de hardware, como processadores, memória, subsistemas de armazenamento e energia dos servidores de Fujitsu PRIMERGY. Ao usar os padrões de design UX do Windows Admin Center e controles da interface do usuário, a Fujitsu nos ajudou a realizar a visão de informações de ponta a ponta de funções e serviços do servidor, para sistema operacional e gerenciamento de hardware por meio da plataforma do Windows Admin Center. [Saiba mais sobre as extensões da Fujitsu e sua experiência de desenvolvimento](case-studies/fujitsu.md).

![Extensão ServerView da Fujitsu](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

Extensão de XClarity integrador da Lenovo leva o gerenciamento de hardware para o próximo nível, integração perfeita com várias experiências de dentro do Windows Admin Center. A solução de integrador XClarity fornece uma visão geral de todos os servidores da Lenovo e as extensões de ferramentas diferentes para fornecer detalhes de hardware se você estiver conectado a um único servidor, cluster de failover ou um cluster hiperconvergente. [Saiba mais sobre a extensão do Lenovo XClarity integrador](case-studies/lenovo.md).

![Extensão do Lenovo](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

Armazenamento puro fornece enterprise, soluções de armazenamento de dados de todos os flash que fornecem uma arquitetura centrados em dados para acelerar seus negócios como uma vantagem competitiva. A extensão de armazenamento puro para Windows Admin Center fornece uma exibição de painel único em produtos FlashArray pura e capacita os usuários a realizar tarefas de monitoramento, exibir as métricas de desempenho em tempo real e gerenciar volumes de armazenamento e iniciadores por meio de uma única interface do usuário experiência. [Saiba mais sobre sua experiência de desenvolvimento e de extensões do puro](case-studies/purestorage.md).

![Extensão de armazenamento puro](../media/extensibility-overview/purestorage-extension.png)

### <a name="squared-up"></a>Squared Up

A Squared Up fornece a melhor experiência de monitoramento da classe com base no System Center Operations Manager e integração com o Azure Log Analytics, o Application Insights e outras soluções de monitoramento. A [extensão quadradas backup](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) traz dados históricos de desempenho e topologias de aplicativos em tempo real e dependências para o contexto do servidor e o gerenciamento de cluster que fornece o Windows Admin Center e clientes iniciais tem aclamados o valor da trazendo dados enorme de muitas fontes diferentes em uma única experiência. [Saiba mais sobre a extensão da Squared Up e sua experiência de desenvolvimento](case-studies/squared-up.md).

![Extensão da Squared Up](../media/extensibility-overview/squaredup-extension.png)