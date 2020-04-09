---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Adicionar\-de entrada na descrição da página
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 8d3cc69bde1c9126f97926802b53d049ed1ef501
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859989"
---
# <a name="add-sign-in-page-description"></a>Adicionar\-de entrada na descrição da página


## <a name="to-add-sign-in-page-description"></a>Para adicionar o\-de entrada na descrição da página  
Para adicionar um\-de entrada na descrição da página à página de\-de entrada, use o cmdlet e a sintaxe do Windows PowerShell a seguir.  

![Adicionar Descrição de entrada](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> A cadeia de caracteres para o parâmetro `SignInPageDescriptionText` dá suporte a HTML puro com e sem tags. Portanto, você também pode executar o cmdlet a seguir sem usar a marca &lt;p&gt;.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Depois que o sinal\-na página é personalizado, a personalização tem precedência; Portanto, você deve personalizar o para todos os idiomas aos quais deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Quando você configura o conteúdo localizado, ele deve ser configurado com um país\-menos localidade, por exemplo, "en", antes de configurar o país e a região\-localidade específica, como "en\-US".  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
