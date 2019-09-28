---
title: O failover de teste deve ser tentado após a conclusão da replicação inicial
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ec3ad3994227eb14d1d2e53842c755af76ac538d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364705"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>O failover de teste deve ser tentado após a conclusão da replicação inicial

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="problem"></a>Problema  
*Não houve nenhum failover de teste em pelo menos um mês.*  
  
## <a name="impact"></a>Impacto  
*There não é uma confirmação de que um failover planejado ou não planejado terá sucesso ou as operações de carga de trabalho continuarão corretamente após um failover. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para conduzir um failover de teste.*  
  


