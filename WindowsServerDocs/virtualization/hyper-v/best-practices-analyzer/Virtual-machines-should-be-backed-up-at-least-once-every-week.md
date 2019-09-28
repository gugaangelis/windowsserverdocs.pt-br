---
title: O backup das máquinas virtuais deve ser feito pelo menos uma vez a cada semana
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ec11b067de2c9f8cbb3a17731caa0dc526bf54a0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393240"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>O backup das máquinas virtuais deve ser feito pelo menos uma vez a cada semana

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Erro|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Uma ou mais máquinas virtuais não foram submetidas a backup na última semana.*  
  
## <a name="impact"></a>Impacto  
a perda de dados de @no__t 0Significant poderá ocorrer se a máquina virtual encontrar um problema e um backup recente não existir. Isso afeta as seguintes máquinas virtuais: *  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Schedule um backup das máquinas virtuais para executar pelo menos uma vez por semana. Você poderá ignorar essa regra se essa máquina virtual for uma réplica e sua máquina virtual primária estiver sendo submetida a backup, ou se esta for a máquina virtual primária e sua réplica estiver sendo submetida a backup.*  
  


