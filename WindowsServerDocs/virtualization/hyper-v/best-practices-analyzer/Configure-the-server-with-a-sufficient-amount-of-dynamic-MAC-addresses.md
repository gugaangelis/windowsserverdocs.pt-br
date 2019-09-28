---
title: Configurar o servidor com uma quantidade suficiente de endereços MAC dinâmicos
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: efd1999411187a592cd8d175eb6de25e11605623
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364940"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>Configurar o servidor com uma quantidade suficiente de endereços MAC dinâmicos

>Aplica-se a: Windows Server 2016

o tópico *This destina-se a resolver um problema específico identificado por uma verificação de Analisador de Práticas Recomendadas. Você deve aplicar as informações neste tópico somente a computadores que tiveram o Analisador de Práticas Recomendadas do Hyper-V executados em relação a eles e que estão enfrentando o problema resolvido por este tópico. Para obter mais informações sobre práticas recomendadas e verificações, consulte @ no__t-0 [analisador de práticas recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*O número de endereços MAC dinâmicos disponíveis é baixo.*  
  
## <a name="impact"></a>Impacto  
  
*Quando não há nenhum endereço MAC dinâmico disponível, as máquinas virtuais configuradas para usar um endereço MAC dinâmico não podem ser iniciadas.*  
  
## <a name="resolution"></a>Resolução  
  
*Use o Gerenciador de comutador virtual para exibir e estender o intervalo de endereços dinâmicos.*  
  


