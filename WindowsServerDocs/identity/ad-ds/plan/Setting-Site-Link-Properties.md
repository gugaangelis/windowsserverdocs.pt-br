---
ms.assetid: de054ac2-a386-43ec-a537-c0de21549741
title: Definindo propriedades do link de site
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: e576d28048a7ad1a5d2bc53d692ef9b02a6a7ebd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945586"
---
# <a name="setting-site-link-properties"></a>Definindo propriedades do link de site

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

A replicação entre sites ocorre de acordo com as propriedades dos objetos de conexão. Quando o Knowledge Consistency Checker (KCC) cria objetos de conexão, ele deriva o agendamento de replicação das propriedades dos objetos de link de site. Cada objeto de link de site representa a conexão WAN (rede de longa distância) entre dois ou mais sites.

A configuração das propriedades do objeto link do site inclui as seguintes etapas:

-   Determinando o custo associado a esse caminho de replicação. O KCC usa o custo para determinar a rota menos dispendiosa para a replicação entre dois sites que replicam a mesma partição de diretório.

-   Determinando o agendamento que define os tempos durante os quais a replicação entre sites pode ocorrer.

-   Determinar o intervalo de replicação que define com que frequência a replicação deve ocorrer durante os horários em que a replicação é permitida, conforme definido no agendamento.

## <a name="in-this-guide"></a>Neste guia

-   [Como determinar o custo](../../ad-ds/plan/Determining-the-Cost.md)

-   [Como determinar o cronograma](../../ad-ds/plan/Determining-the-Schedule.md)

-   [Como determinar o intervalo](../../ad-ds/plan/Determining-the-Interval.md)



