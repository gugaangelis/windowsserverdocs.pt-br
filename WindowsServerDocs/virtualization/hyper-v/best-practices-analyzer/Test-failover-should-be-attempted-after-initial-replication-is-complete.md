---
title: O failover de teste deve ser tentado após a conclusão da replicação inicial
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cea7eeaa-c1a7-4f87-89be-d4e1208c546f
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 41d96b33c686631f57cd35e76b64ee3dde206655
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858939"
---
# <a name="test-failover-should-be-attempted-after-initial-replication-is-complete"></a>O failover de teste deve ser tentado após a conclusão da replicação inicial

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="problem"></a>Problema  
*Não houve nenhum failover de teste em pelo menos um mês.*  
  
## <a name="impact"></a>Impacto  
*Não há nenhuma confirmação de que um failover planejado ou não planejado terá sucesso ou as operações de carga de trabalho continuarão corretamente após um failover. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para conduzir um failover de teste.*  
  


