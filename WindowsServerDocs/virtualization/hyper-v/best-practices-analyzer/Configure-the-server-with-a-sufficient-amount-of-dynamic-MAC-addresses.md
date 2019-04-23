---
title: Configurar o servidor com uma quantidade suficiente de endereços MAC dinâmicos
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: a2804519-9790-4006-80b6-e990a8f505fe
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fc444225c38ef7e8605ec328cfe3f8184b2fd307
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870727"
---
# <a name="configure-the-server-with-a-sufficient-amount-of-dynamic-mac-addresses"></a>Configurar o servidor com uma quantidade suficiente de endereços MAC dinâmicos

>Aplica-se a: Windows Server 2016

*Este tópico destina-se a um problema específico identificado por um exame do analisador de práticas recomendadas. Você deve aplicar as informações neste tópico somente a computadores que tiveram o analisador de práticas recomendadas do Hyper-V executados em relação a elas e estão enfrentando o problema abordado por este tópico. Para obter mais informações sobre as práticas recomendadas e varreduras, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*O número de endereços de MAC dinâmicos disponíveis está baixa.*  
  
## <a name="impact"></a>Impacto  
  
*Quando não há endereços MAC dinâmicos estão disponíveis, as máquinas virtuais configuradas para usar um endereço MAC dinâmico não pode ser iniciadas.*  
  
## <a name="resolution"></a>Resolução  
  
*Use o Gerenciador de comutador Virtual para exibir e estender o intervalo de endereços dinâmicos.*  
  


