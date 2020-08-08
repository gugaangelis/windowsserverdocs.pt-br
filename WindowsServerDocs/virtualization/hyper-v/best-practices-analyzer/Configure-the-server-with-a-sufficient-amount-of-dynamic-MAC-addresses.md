---
title: Configurar o servidor com uma quantidade suficiente de endereços MAC dinâmicos
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: b0416bb38e78f62bddf8a7937eb821ee0988286d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87968373"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>Configurar o servidor com uma quantidade suficiente de endereços MAC dinâmicos

>Aplica-se a: Windows Server 2016

*Este tópico destina-se a resolver um problema específico identificado por uma verificação de analisador de práticas recomendadas. Você deve aplicar as informações neste tópico somente a computadores que tiveram o Analisador de Práticas Recomendadas do Hyper-V executados em relação a eles e que estão enfrentando o problema resolvido por este tópico. Para obter mais informações sobre práticas recomendadas e verificações, consulte* [analisador de práticas recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*O número de endereços MAC dinâmicos disponíveis é baixo.*

## <a name="impact"></a>Impacto

*Quando não há nenhum endereço MAC dinâmico disponível, as máquinas virtuais configuradas para usar um endereço MAC dinâmico não podem ser iniciadas.*

## <a name="resolution"></a>Resolução

*Use o Gerenciador de comutador virtual para exibir e estender o intervalo de endereços dinâmicos.*



