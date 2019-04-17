---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: "Personalização para localização"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ac206d5aa8af970b65a014955ac66a8cf2835eb6
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="customization-for-localization"></a>Personalização para localização 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

É possível localizar o conteúdo da web em idiomas diferentes do inglês. Lembre-se das seguintes considerações quando você localizar.  
  
Depois que o conteúdo é personalizado, a personalização tem precedência; Portanto, você deve personalizar para todos os idiomas que você deseja dar suporte. Todo o conteúdo personalizado leva um parâmetro de localidade. Quando você configura o conteúdo localizado, configurá-lo com uma localidade country\ menos pela primeira vez, por exemplo, "en", antes de você configurar um país e da localidade region\ específicas, como "en\-us".  
  
Veja a seguir alguns exemplos de código adicional.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
Veja a seguir alguns exemplos de código adicional.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Se você quiser personalizar o conteúdo da web para idiomas diferentes do inglês que exige a entrada de Unicode, recomendamos que você use o Windows PowerShell ISE. Para obter informações adicionais, consulte [apresentando o Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md) 
