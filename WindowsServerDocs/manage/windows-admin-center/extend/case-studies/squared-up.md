---
title: Estudo de caso SDK do Windows Admin Center - quadrado para cima
description: Estudo de caso SDK do Windows Admin Center - quadrado para cima
ms.technology: extend
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 05/23/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ab0a7bdcf2388ffc867763c04e183b7388fd13e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863937"
---
# <a name="squared-up-extension"></a>Extensão de quadrado

## <a name="bringing-scom-based-monitoring-server-dependency-visibility-and-external-data-insights-into-windows-admin-center"></a>Trazendo o monitoramento do SCOM, visibilidade de dependência de servidor e Windows Admin Center percepções de dados externa

Quadrado backup foi fundada com a visão de como usar a visualização de dados para ajudar a resolver os desafios de complexidade de TI da empresa. Experiência de software somente da interface do usuário exclusivo, leve ao quadrado para cima, na parte superior da Microsoft poderosa plataforma do System Center Operations Manager, bem como integração com outras fontes de dados - do Log Analytics do Azure da Microsoft, Application Insights e sistema Center Service Manager para produtos de terceiros, como ServiceNow, Splunk e muito mais - para fornecer visibilidade em larga escala empresarial instalações de aplicativo e infraestrutura locais e em ambientes de nuvem híbrida.

> <cite>"Estamos já vem utilizando intensamente Windows Admin Center em todo seu Technical Preview e tem sido um enorme impacto no já, realmente ajudar a resolver desafios como nossos engenheiros obtendo acesso fácil aos nossos laboratórios de configuração, e nossa intenção é torná-lo em nosso gerenciamento primário console depois que ele atinge a versão completa. Adoramos o potencial da integração com o backup de quadrado e a capacidade de trazer à tona todos os nossos dados em um único lugar."</cite>
>
> -- David Acevedo, eu / S especialista em energia NuStar L.P.

Do clientes quadrado gerenciam centenas, milhares de muitas vezes, de servidores Windows e o aplicativo diversificado portfólios entregues-los e ao quadrado para cima e da Microsoft estão em uma missão de trazer as equipes de TI a melhor na web moderna, de rápida, interface do usuário para fornecer as informações que precisam. Como resultado, a equipe no quadrado para cima imediatamente viu um alinhamento empolgante com Windows Admin Center, que apresenta os mesmos valores e entidades de segurança para a próxima geração de administração do Windows Server. Em particular, a equipe acredita-se que os dados de desempenho a longo prazo, insights de dependência de servidor em tempo real e contexto do aplicativo fornecido pela quadrado backup seriam complementam perfeitamente o o elegante elegantes, em tempo real de dados e recursos de gerenciamento de servidor fornecidos pelo Do Windows Admin Center.

![Extensão de quadrado](../../media/extend-case-study-squared-up/squared-up-1.png)

> <cite>"Como uma organização que gerenciar um espaço de servidor de grande escala, ao quadrado backup / integração de Windows Admin Center é o casamento perfeito de nossas ferramentas localizada e centralizada e coisas como sendo capaz de gerar um servidor diretamente no modo de manutenção de dentro Windows Admin Center são ótimos wins pouco para que possamos"</cite>
>
> -– Kip Granson, administrador de sistemas de virtualização na Purdue University

Armado com uma visão clara do que desejam apresentar os dados diretamente dentro do Windows Admin Center, backup de quadrado trabalhou com a versão de visualização privada antecipada do SDK do Windows Admin Center e achei simples, bem documentado e flexível.

Usando o SDK do Windows Admin Center, quadrado backup foi capaz de criar uma extensão que incorpora dinamicamente relevantes ao quadrado-se a experiência de modos de exibição dentro do Windows Admin Center. Por exemplo, dentro do contexto de um determinado servidor ou cluster, modos de exibição ao quadrado backup serão inseridos automaticamente para fornecido maior visibilidade. Modos de exibição incluem as tendências históricas de desempenho e métricas de capacidade (como CPU, memória e disco), a hospedagem de pilha (virtualização de plataforma ou o datacenter de nuvem), componentes de aplicativos, como bancos de dados SQL e serviços e até mesmo a análise de log baseado em nuvem e os dados ITSM.

![Extensão de quadrado](../../media/extend-case-study-squared-up/squared-up-2.png)

Quadrado para cima e para Windows Admin Center compartilham uma web moderna arquitetura e design de costume, que permitiu uma simple integração técnica e uma experiência perfeita ao usuário. Com cada vez mais se tornando a norma de administração baseado na web, acreditamos que esse método de integração entre sistemas diferentes é a chave para desbloquear uma experiência de administração modernos e unificada.

> <cite>"Podemos ver Windows Admin Center como a última geração de administração do Windows Server modernos, portanto, tem sido uma ótima experiência para que possamos trabalhar tão intimamente com a equipe e o fato de que ele trabalhe com essas velocidade, entusiasmo, flexibilidade e dentro de tal fundamentalmente paradigma de desenvolvimento moderno tornou-los uma excelente opção com a maneira como, como uma empresa de desenvolvimento de software lean manufacturing, ágil e rápido, trabalhamos sozinhos. "</cite>
>
> – Richard Benwell, arquiteto de produto no quadrado para cima

Desse alinhamento natural, a equipe de desenvolvimento no quadrado backup foi capaz de avançar rapidamente para uma integração de protótipo exibindo nativamente ao quadrado para cima em uma experiência Windows Admin Center e para fazer isso nas mãos de seus próprios pioneiras, técnicas os clientes de visualização. Reações dos clientes, era claro de imediato que a história foi vencedor.

> <cite>"Um dos principais desafios de manter o serviço pendente em todo o ambiente de servidores mais de 3.500 é unificando nosso diversificado paisagem de gerenciamento e monitoramento de ferramentas e, portanto, a integração entre o quadrado para cima e para Windows Admin Center - que traz juntos tantos dados, de muitas fontes diferentes, em um único console – são enormes para nós."</cite>
>
> – Martin Ehrnst, líder técnico do Azure na Intility/s

Com esse tipo de entusiasmo do quadrado clientes já e com milhares de ótimos recursos novos ainda para chegar à do Windows Admin Center, quadrado backup é imensamente empolgado para o futuro dessa integração e as possibilidades awesome-lo abre para seus clientes e seus jornada para uma verdadeira-de-único painel sua operações para gerenciamento de TI.

O quadrado para cima / integração de Windows Admin Center está atualmente em Beta; Se você gostaria de acessar, faça check-out [página dedicada da quadrado para cima](https://squaredup.com/product/honolulu/windows-admin-center-extension/?utm_source=microsoft-wac&utm_medium=public-relations&utm_campaign=honolulu) para obter mais detalhes. Se sua organização usa o Microsoft System Center Operations Manager e você ainda não tiver quadrado para cima (que é essencial para a extensão trabalhar), em seguida, você também pode obter suas mãos em uma avaliação gratuita de 30 dias com recursos completos, do mesmo local. 
