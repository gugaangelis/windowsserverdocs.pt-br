---
ms.assetid: 96a6749c-6c9f-4f2f-ad0a-51272d282ace
title: Determinando o intervalo
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: a5d19c66372ef577708bb759cfaefab845d882c5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943059"
---
# <a name="determining-the-interval"></a>Determinando o intervalo

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Você deve definir a propriedade intervalo de replicação do link de site para indicar com que frequência deseja que a replicação ocorra durante os momentos em que a agenda permite a replicação. Por exemplo, se a agenda permitir a replicação entre 02:00 horas e 04:00 horas e o intervalo de replicação for definido para 30 minutos, a replicação poderá ocorrer até quatro vezes durante o horário agendado. O intervalo de replicação padrão é de 180 minutos ou 3 horas. O intervalo mínimo é de 15 minutos.

Considere os seguintes critérios para determinar com que frequência a replicação ocorre dentro da janela de agendamento:

-   Um pequeno intervalo diminui a latência, mas aumenta a quantidade de tráfego de WAN (rede de longa distância).

-   Para manter as partições do diretório de domínio atualizadas, a baixa latência é preferida.

Com uma estratégia de replicação de loja e encaminhamento, é difícil determinar quanto tempo uma atualização de diretório pode demorar para ser replicada para cada controlador de domínio. Para fornecer uma estimativa conservadora de latência máxima, execute estas tarefas:

-   Crie uma tabela de todos os sites em sua rede, conforme mostrado no exemplo a seguir:

    |Sites|Seattle|Boston|Los Angeles|Nova York|Washington, D.C.|
    |---------|-----------|----------|---------------|------------|--------------------|
    |Seattle|0,25|||||
    |Boston||0,25||||
    |Los Angeles|||0,25|||
    |Nova York||||0,25||
    |Washington, D.C.|||||0,25|

    Uma latência de pior caso em um site é estimada como 15 minutos.

-   No agendamento de replicação, determine a latência máxima de replicação que é possível em qualquer link de site que conecta dois sites de Hub.

    Por exemplo, se a replicação ocorrer entre Seattle e Nova York a cada três horas, o atraso máximo para replicação entre esses sites será de três horas. Se o atraso de replicação entre a Nova York e Seattle for o atraso agendado mais longo entre todos os sites de Hub, a latência máxima entre todos os hubs será de três horas.

-   Para cada site de Hub, crie uma tabela com as latências máximas entre o site de Hub e qualquer um de seus sites de satélite.

    Por exemplo, se a replicação ocorrer entre Nova York e Washington, D.C., a cada quatro horas e esse for o atraso de replicação mais longo entre a Nova York e qualquer um de seus sites de satélite, a latência máxima entre a Nova York e seus satélites será de quatro horas.

-   Combine essas latências máximas para determinar a latência máxima de toda a rede.

    Por exemplo, se a latência máxima entre Seattle e seu site de satélite em Los Angeles for um dia, a latência máxima de replicação para esse conjunto de links (Washington, D.C.-Nova York-Seattle-Los Angeles) é 31 horas, ou seja, 4 (Washington, D.C.-Nova York) + 3 (Nova York-Seattle) + 24 (Seattle-Los Angeles), conforme mostrado na tabela a seguir.

    |Sites|Seattle|Boston|Los Angeles|Nova York|Washington, D.C.|
    |---------|-----------|----------|---------------|------------|--------------------|
    |Seattle|0,25|4 + 3|24, 0|3.00|4 + 3|
    |Boston||0,25|4 + 3 + 24|4,00|4,00|
    |Los Angeles|||0,25|24 + 3|24 + 3 + 4|
    |Nova York||||0,25|4,00|
    |Washington, D.C.|||||0,25|



