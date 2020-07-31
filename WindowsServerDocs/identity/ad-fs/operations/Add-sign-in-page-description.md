---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Adicione uma descrição na página de entrada
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 918fdce52972edc0b7b4ccba76f8b65b6d91786f
ms.sourcegitcommit: 67d9c51e396c8f937f8704a25e66fea8c5fae81a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87441537"
---
# <a name="add-sign-in-page-description"></a>Adicionar \- Descrição da página de entrada


## <a name="to-add-sign-in-page-description"></a>Para adicionar \- Descrição da página de entrada  
Para adicionar uma descrição de página de entrada \- à página de entrada \- , use o cmdlet e a sintaxe do Windows PowerShell a seguir.  

![Adicionar Descrição de entrada](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> A cadeia de caracteres para o parâmetro `SignInPageDescriptionText` dá suporte a HTML puro com e sem tags. Portanto, você também pode executar o cmdlet a seguir sem usar &lt; a &gt; marca p.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Depois que a \- página de entrada é personalizada, a personalização tem precedência; portanto, você deve personalizar para todos os idiomas aos quais você deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Quando você configura o conteúdo localizado, ele deve ser configurado com um país \- menos localidade primeiro, por exemplo, "en", antes de configurar a localidade específica de país e região, como \- "en \- US".  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
