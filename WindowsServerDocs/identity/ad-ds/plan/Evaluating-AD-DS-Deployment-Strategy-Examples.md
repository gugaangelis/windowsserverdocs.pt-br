---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: "Avaliando exemplos de estratégia de implantação do AD DS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 4cac1e344cfed078927f2945048807d686deebd1
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Avaliando exemplos de estratégia de implantação do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considere o seguinte exemplo de uma empresa fictícia Contoso produtos farmacêuticos, que está implantando serviços de domínio do Active Directory (AD DS) em seu ambiente. O ambiente da Contoso consiste em quatro domínios. O nível funcional da floresta é o Windows Server 2003. A ilustração a seguir mostra a estrutura de domínio atual para a organização Contoso.  
  
![Estratégia de implantação do AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Após analisar é ambiente existente e identificar suas metas de implantação, Contoso estabeleceu a seguinte estratégia de implantação do AD DS:  
  
-   Atualize domínios do Windows Server 2003 para domínios do Windows Server 2008.  
  
-   Habilite recursos avançados do AD DS acionando os níveis funcionais de domínio e da floresta para o Windows Server 2008.  
  
-   Reestruturação domínio africa.concorp.contoso.com dentro da floresta para consolidar esse domínio com o domínio emea.concorp.contoso.con.  
  
Aumentar o nível funcional da floresta para o Windows Server 2008 permitirá que Contoso tirar total proveito dos novos recursos do AD DS. Reestruturação os domínios na floresta, conforme mostrado na ilustração a seguir, reduz a quantidade de administração que é necessária para gerenciar os domínios.  
  
![Estratégia de implantação do AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


