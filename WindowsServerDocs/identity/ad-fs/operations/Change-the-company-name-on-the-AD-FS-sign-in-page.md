---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: Alterar o nome da empresa na página de entrada AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c4991b27f104cb96f55f09fa9467f2b93868b910
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407736"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Alterar o nome da empresa na página de entrada AD FS
 
Para alterar o nome da empresa que é exibida na página assinar @ no__t-0in, use o cmdlet e a sintaxe do Windows PowerShell a seguir. Esse valor é definido por padrão usando o valor do nome de exibição de Serviço de Federação que você digitou durante a configuração.  

![alterar nome](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> Você também pode usar o Ambiente de Script Integrado do Windows PowerShell \(ISE @ no__t-1 para alterar o nome da empresa. Usando o ISE do Windows PowerShell, você pode exibir o conteúdo em um ambiente Unicode @ no__t-0compliant. Para obter informações adicionais, consulte [Apresentando o Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
  
