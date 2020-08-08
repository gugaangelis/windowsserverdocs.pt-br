---
ms.assetid: 38bbc002-a8fa-4211-9328-4ef67fca0acf
title: Personalização para localização
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 41a540b43fde264718ca7aa81ba0e48184cf1265
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956383"
---
# <a name="customization-for-localization"></a>Personalização para localização

É possível localizar conteúdo da Web em idiomas diferentes do inglês. Lembre-se das seguintes considerações quando localizar.

Depois de personalizar o conteúdo, a personalização terá prioridade; portanto, você deve personalizar para todos os idiomas aos quais deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Ao configurar conteúdo localizado, configure-o com um país \- menos localidade primeiro, por exemplo, "en", antes de configurar uma localidade específica de país e região, como \- "en \- US".

Seguem alguns exemplos de código adicional.

```powershell
Set-AdfsWebTheme -TargetName default -Logo @{Locale="";Path="c:\contoso.png"}
Set-AdfsWebTheme -TargetName default -Illustration @{Locale="";Path="c:\illustration.png"}
```

Seguem alguns exemplos de código adicional.

```powershell
Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" –locale "en"

Set-AdfsGlobalWebContent -ErrorPageDescriptionText "Il s'agit de description de page erreur de Contoso" –locale "fr"
```

Se você quiser personalizar o conteúdo da Web para idiomas diferentes do inglês que exijam a entrada do Unicode, recomendamos o uso do ISE do Windows PowerShell. Para obter informações adicionais, consulte [Introducing the ISE do Windows PowerShell](/previous-versions/mt707506(v=msdn.10)).

## <a name="additional-references"></a>Referências adicionais

[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)
