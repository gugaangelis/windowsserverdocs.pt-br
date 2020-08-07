---
title: Mais de um adaptador de rede deve estar disponível
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 57abcbbc796ab2664d30ca9ff63c1e41f47c17a4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950240"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Mais de um adaptador de rede deve estar disponível

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Erro|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*Esse servidor é configurado com um adaptador de rede, que deve ser compartilhado pelo sistema operacional de gerenciamento e por todas as máquinas virtuais que exigem acesso a uma rede física.*

## <a name="impact"></a>Impacto

*O desempenho da rede pode ser degradado no sistema operacional de gerenciamento.*

## <a name="resolution"></a>Resolução

*Adicione mais adaptadores de rede a este computador. Para reservar um adaptador de rede para uso exclusivo pelo sistema operacional de gerenciamento, não o configure para uso com uma rede virtual externa.*

Para obter informações sobre como adicionar um adaptador de rede ao computador, consulte a documentação do computador ou adaptador de rede. Em seguida, para reservar exclusivamente para o sistema operacional de gerenciamento, não conecte-o a um comutador virtual.



