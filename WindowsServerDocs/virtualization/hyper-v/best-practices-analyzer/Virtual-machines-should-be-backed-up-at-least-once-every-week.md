---
title: Máquinas virtuais deve ser feitas pelo menos uma vez a cada semana
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7dbd3dfc-c873-4a77-89f7-3166e18d9531
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e079e3cb225ec9c712233bbf3efc85bb6f09b218
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59826747"
---
# <a name="virtual-machines-should-be-backed-up-at-least-once-every-week"></a>Máquinas virtuais deve ser feitas pelo menos uma vez a cada semana

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Uma ou mais máquinas virtuais não têm foi feitas backup da semana anterior.*  
  
## <a name="impact"></a>Impacto  
*Perda significativa de dados pode ocorrer se a máquina virtual encontrar um problema e um backup recente não existe. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Agende um backup das máquinas virtuais para executar pelo menos uma vez por semana. Você pode ignorar esta regra se essa máquina virtual é uma réplica e a sua máquina virtual primária está sendo feita backup, ou se essa for a máquina virtual primária e sua réplica está sendo feita backup.*  
  


