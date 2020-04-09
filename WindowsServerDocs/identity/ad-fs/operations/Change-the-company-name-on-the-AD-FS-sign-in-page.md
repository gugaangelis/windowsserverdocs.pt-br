---
ms.assetid: 28043fc4-a34d-4710-ac3b-5c9d4d6a895c
title: Alterar o nome da empresa na página de entrada AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 378979825c0e8e505c3996cf8cbcdb471a0c7d79
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859959"
---
# <a name="change-the-company-name-on-the-ad-fs-sign-in-page"></a>Alterar o nome da empresa na página de entrada AD FS
 
Para alterar o nome da empresa que é exibida na página\-de entrada, use o cmdlet e a sintaxe do Windows PowerShell a seguir. Esse valor é definido por padrão usando o valor do nome de exibição de Serviço de Federação que você digitou durante a configuração.  

![alterar nome](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom1.png)
  
  
    Set-AdfsGlobalWebContent –CompanyName "Contoso Corp"  
 
  
> [!NOTE]  
> Você também pode usar o\) \(ISE Ambiente de Script Integrado do Windows PowerShell para alterar o nome da empresa. Usando o ISE do Windows PowerShell, você pode exibir o conteúdo em um ambiente em conformidade com\-Unicode. Para obter informações adicionais, consulte [Apresentando o Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
  
