---
title: Uma SAN virtual deve ser associada a um adaptador de barramento de host físico
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 344b3b60c0d4613b121ef7ed17447810e0d6293d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87965583"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Uma SAN virtual deve ser associada a um adaptador de barramento de host físico

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|


Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>**Problema**
*Uma SAN (rede de área de armazenamento) virtual foi configurada sem uma associação a um adaptador de barramento de host (HBA).*

## <a name="impact"></a>**Impacto**
*Uma máquina virtual não será iniciada quando estiver configurada com um adaptador de Fibre Channel virtual conectado a uma SAN virtual configurada incorretamente. Isso afeta as seguintes SANs virtuais:*


\<list of virtual SANs>


## <a name="resolution"></a>**Resolução**
*Reconfigure a rede SAN virtual conectando-a a um adaptador de barramento de host.*





