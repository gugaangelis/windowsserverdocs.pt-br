---
title: A interface de equipe associada a um comutador virtual deve estar no modo padrão
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 8c118e1e-865f-4cff-acdc-7c35e45d5da9
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: fe19de5dd380d08c01c917da9d4e2ef9465de042
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854599"
---
# <a name="the-team-interface-bound-to-a-virtual-switch-should-be-in-default-mode"></a>A interface de equipe associada a um comutador virtual deve estar no modo padrão

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Alguns comutadores virtuais estão associados a uma interface de equipe, mas a interface de equipe não passa o tráfego em todas as VLANs para os comutadores virtuais.*  
  
## <a name="impact"></a>**Causa**  
*Os seguintes comutadores virtuais não podem ter acesso a todas as VLANs: \n{0}*  
  
## <a name="resolution"></a>**Resolução**  
*Use Gerenciador do Servidor ou o cmdlet do Windows PowerShell Set-NetLbfoTeamNic para redefinir a interface de equipe para o modo padrão.*  
  


