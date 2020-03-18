---
title: Estudo de caso do SDK do centro de administração do Windows – quadrado para cima
description: Estudo de caso do SDK do centro de administração do Windows – quadrado para cima
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 620d3d9f4b5c3638d49fe9141e83ebdcb9eb245c
ms.sourcegitcommit: 5197a87e659589bcc8d2a32069803ae736b02892
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/16/2020
ms.locfileid: "79429300"
---
# <a name="squared-up-extension"></a>Extensão de quadrado para cima

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>Trazendo o monitoramento baseado em SCOM, a visibilidade de dependência do servidor e as informações de dados externas no centro de administração do Windows

O enquadramento foi fundada com a visão de como usar a visualização de dados para ajudar a resolver os desafios da complexidade de ti empresarial. O software único, leve e exclusivo da interface do usuário se baseia na plataforma de System Center Operations Manager avançada da Microsoft, bem como na integração com fontes de dados adicionais – da própria Log Analytics do Azure, Application Insights e System da Microsoft. Centralize o Service Manager a produtos de terceiros, como ServiceNow, Splunk e muito mais, para proporcionar visibilidade em grandes dimensões de aplicativos e infraestrutura corporativa, tanto localmente quanto em ambientes de nuvem híbridas.

> <cite>"Estamos muito utilizando o centro de administração do Windows ao longo de sua visualização técnica e já foi um grande problema, ajudando realmente a resolver os desafios como nossos engenheiros que obtêm acesso fácil aos nossos laboratórios de configuração e pretendemos torná-lo o nosso principal gerenciamento console quando ele atingir a versão completa. Adoramos o potencial da integração com o enquadrado e a capacidade de trazer todos os nossos dados em um único lugar. "</cite>
>
> --David Acevedo, especialista de e/S no NuStar Energy L.P.

Os clientes do quadrante gerenciam centenas, geralmente milhares de servidores Windows e os diversos portfólios de aplicativos fornecidos por eles, e os dois enquadrados e a Microsoft estão em uma missão de trazer as equipes de ti as melhores na interface do usuário da Web rápida e moderna para fornecer as informações necessárias. Como resultado, a equipe ao quadrado imediatamente viu um alinhamento empolgante com o centro de administração do Windows, que traz os mesmos valores e entidades para a próxima geração da administração do Windows Server. Em particular, a equipe acredita que os dados de desempenho de longo prazo, as ideias de dependência do servidor em tempo real e o contexto do aplicativo na superfície para cima complementariam perfeitamente os recursos de gerenciamento de dados e de servidor em tempo real, bem como os mais elegantes, fornecidos por Centro de administração do Windows.

![Extensão de quadrado para cima](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>"Como uma organização que gerencia um espaço de servidor em grande escala, a integração com o centro de administração do Windows é o casamento perfeito de nossas ferramentas localizadas e centralizadas, como a capacidade de gerar um servidor diretamente no modo de manutenção no centro de administração do Windows, que é excelente para nós"</cite>
>
> -– Gnorar Granson, administrador de sistemas de virtualização na Purdue University

Munido de uma visão clara de querer apresentar esses dados diretamente no centro de administração do Windows, o enquadrado funcionou com a versão prévia privada anterior do SDK do centro de administração do Windows e descobriu que ele é simples, bem documentado e flexível.

Usando o SDK do centro de administração do Windows, o enquadrado foi capaz de criar uma extensão que incorpora dinamicamente exibições relevantes ao quadrado dentro da experiência do centro de administração do Windows. Por exemplo, dentro do contexto de um servidor ou cluster específico, as exibições em quadrado são inseridas automaticamente para a visibilidade estendida fornecida. Os modos de exibição incluem tendências históricas de desempenho e métricas de capacidade chave (como CPU, memória e disco), pilha de hospedagem (plataforma de nuvem ou virtualização de Datacenter), componentes de aplicativos como bancos de dados SQL e serviços e até mesmo log Analytics baseado em nuvem e dados de ITSM.

![Extensão de quadrado para cima](../../media/extend-case-study-squared-up/squared-up-2.png)

Enquadrado e centro de administração do Windows compartilham uma arquitetura da Web moderna e design costume, que habilitou uma integração técnica simples e uma experiência de usuário tranqüila. Com a administração baseada na Web se tornando a norma, acreditamos que esse método de integração entre diferentes sistemas é a chave para desbloquear uma experiência de administração moderna e unificada.

> <cite>"Vemos o centro de administração do Windows como a vanguarda da administração moderna do Windows Server, portanto, é uma ótima experiência para que possamos trabalhar de maneira bem próxima com a equipe e o fato de que estão trabalhando com essa velocidade, entusiasmo, flexibilidade e dentro de tal fundamental os paradigmas de desenvolvimento modernos tornaram-se uma ótima opção com a maneira como nós, como uma empresa de desenvolvimento de software rápida, ágil e acelerada, trabalhamos por si só. "</cite>
>
> --Richard Benwell, arquiteto de produto ao quadrado para cima

Desse alinhamento natural, a equipe de desenvolvimento no enquadrado foi capaz de progredir rapidamente para uma integração com protótipos exibindo de forma nativa na experiência do centro de administração do Windows e para colocá-la em mãos de seus próprios pioneiros, técnicos Visualizar clientes. Das reações dos clientes, ficou imediatamente claro que a história era vencedora.

> <cite>"Um dos principais desafios de manter o serviço pendente em nosso ambiente de mais de 3.500 servidores é unificando nosso variado panorama de ferramentas de gerenciamento e monitoramento e, portanto, a integração entre o enquadramento e o centro de administração do Windows – que traz juntos, tantos dados, de tantas fontes distintas, em um único console – são enormes para nós. "</cite>
>
> --Martin Ehrnst, líder técnico do Azure em Intility A/S

Com esse tipo de entusiasmo dos clientes de enquadramento já e com toneladas de novos recursos excelentes ainda para o centro de administração do Windows, o enquadrado está muito entusiasmado com o futuro dessa integração e pelas possibilidades impressionantes que ele abre para seus clientes e seus jornada para um verdadeiro painel único de vidro para o gerenciamento de operações de ti.

A integração do centro de administração do Windows no enquadramento está em versão beta; Se você quiser acesso, consulte a [página dedicada do quadrado para cima](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu) para obter mais detalhes. Se sua organização usa o Microsoft System Center Operations Manager e você ainda não tem um quadrado para cima (que é essencial para que a extensão funcione), você também pode obter suas mãos em uma avaliação gratuita de 30 dias e completa do mesmo local. 
