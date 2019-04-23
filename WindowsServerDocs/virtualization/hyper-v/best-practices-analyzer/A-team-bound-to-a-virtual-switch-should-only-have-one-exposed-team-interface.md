---
title: Uma equipe associada a um comutador virtual deve ter apenas uma interface exposta de equipe
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 108bbec1439959bb7ab4475b59c7231653952ea8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59838457"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Uma equipe associada a um comutador virtual deve ter apenas uma interface exposta de equipe

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Um ou mais comutadores virtuais estão associados a uma equipe que tem várias interfaces de equipe.*  
  
## <a name="impact"></a>**Impacto**  
*Os comutadores virtuais a seguir podem não ter acesso para VLANs e a largura de banda usada por outras interfaces de equipe:*  
  
\<lista de comutadores virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Use o cmdlet do Windows PowerShell NetLbfoTeamNic remover para remover todas as interfaces da equipe da equipe que não seja a interface de equipe padrão.*  
  


