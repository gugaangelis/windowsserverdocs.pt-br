---
title: Extensões para o Windows Admin Center
description: Extensões para o SDK do Windows Admin Center (Project Honolulu)
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 09/17/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: ee8c0203be25b30f173b1887de506844d5b58738
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406912"
---
# <a name="extensions-for-windows-admin-center"></a>Extensões para o Windows Admin Center

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

O Windows Admin Center foi criado como uma plataforma extensível para permitir aos parceiros e desenvolvedores aproveitar as funcionalidades existentes no Windows Admin Center, integrar com outros produtos de administração de TI e soluções, e fornecer valor adicional para clientes. Cada ferramenta e solução no Windows Admin Center é criada como uma extensão usando os mesmos recursos de extensibilidade disponíveis para parceiros e desenvolvedores. Dessa forma, você poder criar ferramentas tão eficientes como as que estão disponíveis no Windows Admin Center hoje.

As extensões do Windows Admin Center são criadas usando tecnologias da Web modernas, incluindo o HTML5, CSS, Angular, TypeScript e jQuery, e podem gerenciar os servidores de destino pelo PowerShell ou WMI. Você também pode gerenciar servidores de destino, serviços ou dispositivos através de diferentes protocolos, como o REST ao aproveitar um plug-in de gateway do Windows Admin Center.

## <a name="why-you-should-consider-developing-an-extension-for-windows-admin-center"></a>Por que você deve considerar a desenvolver uma extensão para o Windows Admin Center

Este é o valor que você pode trazer para seu produto e clientes ao desenvolver extensões para o centro de administração do Windows:

- **Integre com as ferramentas do centro de administração do Windows:** Integre seus produtos e serviços com as ferramentas de gerenciamento de servidor e cluster no centro de administração do Windows e forneça experiências unificadas e diretas de monitoramento, gerenciamento e solução de problemas para seus clientes.
- **Aproveite os recursos de segurança, identidade e gerenciamento da plataforma:** Habilite o suporte a Azure Active Directory (AAD), autenticação multifator, RBAC (controle de acesso baseado em função), registro em log, auditoria para seu produto e serviços aproveitando os recursos da plataforma centro de administração do Windows para atender aos requisitos complexos de hoje Organizações de ti.
- **Desenvolva usando as tecnologias da Web mais recentes:** Crie rapidamente experiências de usuário impressionantes usando tecnologias da Web modernas, incluindo HTML5, CSS, angular, TypeScript e jQuery, bem como controles avançados e poderosos da interface do usuário incluídos no SDK do centro de administração do Windows.
- **Estenda o âmbito do produto:** Torne-se parte do novo ecossistema do centro de administração do Windows, com a preocupação com nossa base de clientes em rápido crescimento e aproveite o impulso de lançamento do Windows Server 2019 mais tarde neste ano.

## <a name="start-developing-with-the-windows-admin-center-sdk"></a>Comece a desenvolver com o SDK do centro de administração do Windows

É fácil começar a usar o desenvolvimento do centro de administração do Windows!  O código de exemplo pode ser encontrado para [ferramentas](develop-tool.md), [solução](develop-solution.md)e tipos de extensão de [plug-in de gateway](develop-gateway-plugin.md) em nossa documentação do SDK. Lá, você aproveitará a CLI do centro de administração do Windows para criar um novo projeto de extensão e seguirá os guias individuais para personalizar seu projeto para atender às suas necessidades.

Fizemos um [Kit de ferramentas de design do SDK](https://github.com/Microsoft/windows-admin-center-sdk/blob/master/WindowsAdminCenterDesignToolkit.zip) do Windows Admin Center disponível para ajudá-lo a simular rapidamente as extensões no PowerPoint usando estilos, controles e modelos de página do centro de administração do Windows. Veja como a extensão pode ser exibida no centro de administração do Windows antes de começar a codificar.

Também temos um código de exemplo hospedado no GitHub: [Ferramentas para desenvolvedores](https://aka.ms/wacsdk) é uma extensão de solução de exemplo que contém uma rica coleção de controles que você pode procurar e usar em sua própria extensão. Ferramentas de desenvolvedor é uma extensão totalmente funcional que pode ser carregada no Windows Admin Center no modo do desenvolvedor.

Consulte os tópicos a seguir para saber mais sobre o SDK e começar:

- [Entenda como funcionam as extensões](understand-extensions.md)
- [Desenvolver uma extensão](developing-extensions.md)
- [Guias](guides.md)
- [Publicar sua extensão](publish-extensions.md)

## <a name="partner-spotlight"></a>Destaque de parceiro

Veja o valor incrível que nossos parceiros começaram a trazer para o ecossistema do Windows Admin Center e teste essas extensões hoje mesmo. Saiba mais sobre [como instalar as extensões](../configure/using-extensions.md) do Windows Admin Center.

### <a name="biitops"></a>BiitOps
A extensão BiitOps Changes fornece controle de alterações para configurações de hardware, software e configuração em suas máquinas virtuais/físicas do Windows Server. A extensão BiitOps Changes mostrará precisamente o que há de novo, o que mudou e o que foi excluído em um único painel para ajudar a acompanhar problemas relacionados à conformidade, confiabilidade e segurança. [Saiba mais sobre a extensão de alterações de BiitOps](case-studies/biitops.md).

![Extensão BiitOps](../media/extensibility-overview/biitops-1.png)

### <a name="dataon"></a>DataON

A data de extensão deve trazer monitoramento, gerenciamento e informações de ponta a ponta em sistemas de armazenamento e infraestrutura hiperconvergentes de dados com base no Windows Server. A extensão deve adicionar um valor exclusivo, como relatórios de dados históricos, mapeamento de disco, alertas de sistema e serviço de chamada de rede por meio de SAN, complementando o servidor do centro de administração do Windows e recursos de gerenciamento de infraestrutura hiperconvergente, por meio de uma experiência unificada. [Saiba mais sobre a extensão DataON MUST e sua experiência de desenvolvimento](case-studies/dataon.md).

![Extensão DataON MUST](../media/extensibility-overview/dataon-must-extension.png)

### <a name="fujitsu"></a>Fujitsu

O ServerView Health e as extensões de integridade do RAID da Fujitsu para o centro de administração do Windows fornecem monitoramento e gerenciamento detalhados de componentes de hardware críticos, como processadores, memória, energia e subsistemas de armazenamento para servidores Fujitsu PRIMERGY. Ao usar os padrões de design UX do Windows Admin Center e controles da interface do usuário, a Fujitsu nos ajudou a realizar a visão de informações de ponta a ponta de funções e serviços do servidor, para sistema operacional e gerenciamento de hardware por meio da plataforma do Windows Admin Center. [Saiba mais sobre as extensões da Fujitsu e sua experiência de desenvolvimento](case-studies/fujitsu.md).

![Extensão ServerView da Fujitsu](../media/extensibility-overview/fujitsu-serverview-extension.png)

### <a name="lenovo"></a>Lenovo

A extensão do integrador do Lenovo XClarity leva o gerenciamento de hardware para o próximo nível, integrando-se diretamente a várias experiências no centro de administração do Windows. A solução de integrador XClarity fornece uma exibição de alto nível de todos os seus servidores Lenovo, e extensões de ferramentas diferentes fornecem detalhes de hardware se você estiver conectado a um único servidor, cluster de failover ou cluster hiperconvergente. [Saiba mais sobre a extensão do integrador do Lenovo XClarity](case-studies/lenovo.md).

![Extensão do Lenovo](../media/extensibility-overview/lenovo-extension.png)

### <a name="pure-storage"></a>Pure Storage

O armazenamento puro fornece soluções de armazenamento de dados empresariais e totalmente flash que fornecem uma arquitetura centrada em dados para acelerar sua empresa para uma vantagem competitiva. A extensão de armazenamento puro para o centro de administração do Windows fornece uma exibição de painel único em produtos FlashArray puros e permite que os usuários realizem tarefas de monitoramento, exibam métricas de desempenho em tempo real e gerenciem volumes de armazenamento e iniciadores por meio de uma única interface do usuário ocorrer. [Saiba mais sobre extensões puras e sua experiência de desenvolvimento](case-studies/purestorage.md).

![Extensão de armazenamento pura](../media/extensibility-overview/purestorage-extension.png)

### <a name="qct"></a>QCT

A extensão do QCT Management Suite complementa o centro de administração do Windows fornecendo monitoramento e gerenciamento de servidor físico para os sistemas certificados do QCT Azure Stack HCI. A extensão do QCT Management Suite exibe informações de hardware do servidor e fornece uma interface do usuário intuitiva do assistente para ajudar a substituir discos físicos com eficiência, ferramentas de log de eventos de hardware e S.M.A.R.T. gerenciamento de disco de previsão baseado. [Saiba mais sobre a extensão do QCT Management Suite](case-studies/qct.md).

![Extensão QCT](../media/extensibility-overview/qct-extension.png)

### <a name="squared-up"></a>Squared Up

A Squared Up fornece a melhor experiência de monitoramento da classe com base no System Center Operations Manager e integração com o Azure Log Analytics, o Application Insights e outras soluções de monitoramento. A [extensão quadradas backup](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-docs&utm_medium=public-relations&utm_campaign=honolulu) traz dados históricos de desempenho e topologias de aplicativos em tempo real e dependências para o contexto do servidor e o gerenciamento de cluster que fornece o Windows Admin Center e clientes iniciais tem aclamados o valor da trazendo dados enorme de muitas fontes diferentes em uma única experiência. [Saiba mais sobre a extensão da Squared Up e sua experiência de desenvolvimento](case-studies/squared-up.md).

![Extensão da Squared Up](../media/extensibility-overview/squaredup-extension.png)