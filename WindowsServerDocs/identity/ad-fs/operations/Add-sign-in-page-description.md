---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Adicione uma descrição na página de entrada
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 033663720750ee2990cbc6eb4dd0c6d9abe1a002
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962630"
---
# <a name="add-sign-in-page-description"></a>Adicionar \- Descrição da página de entrada

## <a name="to-add-sign-in-page-description"></a>Para adicionar \- Descrição da página de entrada
Para adicionar uma descrição de página de entrada \- à página de entrada \- , use o cmdlet e a sintaxe do Windows PowerShell a seguir.

![Adicionar Descrição de entrada](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

```powershell
Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>"
```

> [!IMPORTANT]
> A cadeia de caracteres para o parâmetro `SignInPageDescriptionText` dá suporte a HTML puro com e sem tags. Portanto, você também pode executar o cmdlet a seguir sem usar &lt; a &gt; marca p.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." `

Depois que a \- página de entrada é personalizada, a personalização tem precedência; portanto, você deve personalizar para todos os idiomas aos quais você deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Quando você configura o conteúdo localizado, ele deve ser configurado com um país \- menos localidade primeiro, por exemplo, "en", antes de configurar a localidade específica de país e região, como \- "en \- US".

## <a name="additional-references"></a>Referências adicionais

[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)
