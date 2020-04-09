---
title: Uma equipe associada a um comutador virtual deve ter apenas uma interface de equipe exposta
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 413448945d2598ba36bed646144a43e39a1a3159
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857939"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Uma equipe associada a um comutador virtual deve ter apenas uma interface de equipe exposta

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema
*Um ou mais comutadores virtuais estão associados a uma equipe que tem várias interfaces de equipe.*  
  
## <a name="impact"></a>Impacto
*Os seguintes comutadores virtuais podem não ter acesso a VLANs e largura de banda usada por outras interfaces de equipe:*  
  
\<lista de comutadores virtuais >  
  
## <a name="resolution"></a>Resolução
*Use o cmdlet do Windows PowerShell remove-NetLbfoTeamNic para remover todas as interfaces de equipe da equipe que não seja a interface de equipe padrão.*  
  


