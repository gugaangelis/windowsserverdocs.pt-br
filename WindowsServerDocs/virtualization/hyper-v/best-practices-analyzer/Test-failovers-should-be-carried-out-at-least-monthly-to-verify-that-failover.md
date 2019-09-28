---
title: Os failovers de teste devem ser executados pelo menos mensalmente para verificar se o failover será bem sucedido e se as cargas de trabalho da máquina virtual funcionarão conforme o esperado após o failover
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 57a8aa50-e59e-4a4b-8571-1099d5a8eee4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c7f7c0e1076358ef417b4d98632bd65257989df3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393462"
---
# <a name="test-failovers-should-be-carried-out-at-least-monthly-to-verify-that-failover-will-succeed-and-that-virtual-machine-workloads-will-operate-as-expected-after-failover"></a>Os failovers de teste devem ser executados pelo menos mensalmente para verificar se o failover será bem sucedido e se as cargas de trabalho da máquina virtual funcionarão conforme o esperado após o failover

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016| 
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Não houve nenhum failover de teste em pelo menos um mês.*  
  
## <a name="impact"></a>Impacto  
*There não é uma confirmação de que um failover planejado ou não planejado terá sucesso ou as operações de carga de trabalho continuarão corretamente após um failover. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para conduzir um failover de teste.*  
  


