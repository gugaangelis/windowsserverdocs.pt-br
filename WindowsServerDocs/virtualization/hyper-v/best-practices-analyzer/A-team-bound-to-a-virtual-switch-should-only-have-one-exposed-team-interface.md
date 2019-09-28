---
title: Uma equipe associada a um comutador virtual deve ter apenas uma interface de equipe exposta
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6baa9e4ae900c9b671003872b4eb4589efb2f085
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365403"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Uma equipe associada a um comutador virtual deve ter apenas uma interface de equipe exposta

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Um ou mais comutadores virtuais estão associados a uma equipe que tem várias interfaces de equipe.*  
  
## <a name="impact"></a>**Causa**  
*Os seguintes comutadores virtuais podem não ter acesso a VLANs e largura de banda usada por outras interfaces de equipe:*  
  
\<list de comutadores virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Use o cmdlet do Windows PowerShell remove-NetLbfoTeamNic para remover todas as interfaces de equipe da equipe que não seja a interface de equipe padrão.*  
  


