---
title: A interface de equipe associada a um comutador virtual deve ser em default modo
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 22e5ad0eed6e6ea07a83150762b76163442f2c5c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872157"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>A interface de equipe associada a um comutador virtual deve ser em default modo

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
*Alguns comutadores virtuais estão associados a uma interface de equipe, mas a interface de equipe não passa o tráfego em todas as VLANs para os comutadores virtuais.*  
  
## <a name="impact"></a>**Impacto**  
*Os comutadores virtuais a seguir não podem ter acesso a todas as VLANs: \n{0}*  
  
## <a name="resolution"></a>**Resolução**  
*Use o Gerenciador do servidor ou o cmdlet do Windows PowerShell Set-NetLbfoTeamNic para redefinir a interface de equipe para o modo padrão.*  
  


