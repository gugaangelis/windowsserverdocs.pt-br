---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: Avaliar exemplos de estratégia de implantação do AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 169d0a55f9fb167390c13ac1c89f8d68427f318d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59842107"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Avaliar exemplos de estratégia de implantação do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considere o seguinte exemplo de uma empresa fictícia, Contoso Pharmaceuticals, que está implantando os serviços de domínio do Active Directory (AD DS) em seu ambiente. Ambiente da Contoso consiste em quatro domínios. O nível funcional da floresta é o Windows Server 2003. A ilustração a seguir mostra a estrutura de domínio atual para a organização Contoso.  
  
![Estratégia de implantação do AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Depois de analisar o ambiente existente e identificar suas metas de implantação, a Contoso estabeleceu a seguinte estratégia de implantação do AD DS:  
  
-   Atualize domínios do Windows Server 2003 para domínios do Windows Server 2008.  
  
-   Habilite recursos avançados do AD DS, elevando os níveis funcionais de domínio e floresta para o Windows Server 2008.  
  
-   Reestruture o domínio africa.concorp.contoso.com dentro da floresta para consolidá esse domínio com o domínio emea.concorp.contoso.con.  
  
Elevar o nível funcional de floresta para o Windows Server 2008 permitirá Contoso para aproveitar ao máximo os novos recursos do AD DS. Reestruturar os domínios na floresta, conforme mostrado na ilustração a seguir, reduzirá a quantidade de administração que é necessária para gerenciar os domínios.  
  
![Estratégia de implantação do AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


