---
title: O backup das máquinas virtuais deve ser feito pelo menos uma vez a cada semana
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 6cb425f92926aa1823ed89cd26afccc2d962603d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855029"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>O backup das máquinas virtuais deve ser feito pelo menos uma vez a cada semana

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Error|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Uma ou mais máquinas virtuais não foram submetidas a backup na última semana.*  
  
## <a name="impact"></a>Impacto  
*Uma perda significativa de dados poderá ocorrer se a máquina virtual encontrar um problema e um backup recente não existir. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Agende um backup das máquinas virtuais para ser executado pelo menos uma vez por semana. Você poderá ignorar essa regra se essa máquina virtual for uma réplica e sua máquina virtual primária estiver sendo submetida a backup, ou se esta for a máquina virtual primária e sua réplica estiver sendo submetida a backup.*  
  


