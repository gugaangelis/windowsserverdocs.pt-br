---
ms.assetid: 4f835b82-67b9-428c-b634-ce133cca5113
title: Avaliar exemplos de estratégia de implantação do AD DS
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 3d04530c53150a3222b609a80938d7fdfcdfeff7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402588"
---
# <a name="evaluating-ad-ds-deployment-strategy-examples"></a>Avaliar exemplos de estratégia de implantação do AD DS

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Considere o exemplo a seguir de uma empresa fictícia, Contoso Pharmaceuticals, que está implantando Active Directory Domain Services (AD DS) em seu ambiente. O ambiente da Contoso consiste em quatro domínios. O nível funcional da floresta é o Windows Server 2003. A ilustração a seguir mostra a estrutura de domínio atual para a organização Contoso.  
  
![Estratégia de implantação de AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/3dd79e00-48f8-4927-989c-c55a79caf1be.gif)  
  
Depois de examinar seu ambiente existente e identificar suas metas de implantação, a Contoso estabeleceu a seguinte estratégia de implantação de AD DS:  
  
-   Atualize domínios do Windows Server 2003 para domínios do Windows Server 2008.  
  
-   Habilite recursos de AD DS avançados elevando os níveis funcionais de domínio e floresta para o Windows Server 2008.  
  
-   Reestruture o domínio africa.concorp.contoso.com dentro da floresta para consolidar esse domínio com o domínio EMEA. concorp. contoso. con.  
  
Aumentar o nível funcional da floresta para o Windows Server 2008 permitirá que a contoso Aproveite ao máximo os novos recursos de AD DS. Reestruturar os domínios dentro da floresta, conforme mostrado na ilustração a seguir, reduzirá a quantidade de administração necessária para gerenciar os domínios.  
  
![Estratégia de implantação de AD DS](media/Evaluating-AD-DS-Deployment-Strategy-Examples/1c061755-413d-452d-b121-6910f8555327.gif)  
  


