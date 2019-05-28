---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personalização para localização
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 3cf209756c27c72836c7e2e1e58e84f3f5af2ca9
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66189177"
---
# <a name="customization-for-localization"></a>Personalização para localização 


É possível localizar conteúdo da Web em idiomas diferentes do inglês. Lembre-se das seguintes considerações quando localizar.  
  
Depois de personalizar o conteúdo, a personalização terá prioridade; portanto, você deve personalizar para todos os idiomas aos quais deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Quando você configura o conteúdo localizado, configurá-lo com um país\-menos localidade primeiro, por exemplo, "en", antes de configurar um país e região\-localidade específica, como "en\-us".  
  
Seguem alguns exemplos de código adicional.  
  
    
    Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}  
      
    Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}  

  
Seguem alguns exemplos de código adicional.  
  
 
    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"  
  
  

    Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"  
 
  
Se você quiser personalizar o conteúdo da web em idiomas diferentes do inglês que requerem a entrada de Unicode, recomendamos que você use o Windows PowerShell ISE. Para obter informações adicionais, consulte [Apresentando o Windows PowerShell ISE](https://technet.microsoft.com/library/dd315244.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md) 
