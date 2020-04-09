---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personalização para localização
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: eab7062c6678c0e9f3ef970ef9cff97fa63dd868
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816389"
---
# <a name="customization-for-localization"></a>Personalização para localização 


É possível localizar conteúdo da Web em idiomas diferentes do inglês. Lembre-se das seguintes considerações quando localizar.  
  
Depois de personalizar o conteúdo, a personalização terá prioridade; portanto, você deve personalizar para todos os idiomas aos quais deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Ao configurar conteúdo localizado, configure-o com um país\-menos localidade, por exemplo, "en", antes de configurar um país e uma região\-localidade específica, como "en\-US".  
  
Seguem alguns exemplos de código adicional.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
Seguem alguns exemplos de código adicional.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Se você quiser personalizar o conteúdo da Web para idiomas diferentes do inglês que exijam a entrada do Unicode, recomendamos o uso do ISE do Windows PowerShell. Para obter informações adicionais, consulte [Apresentando o Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md) 
