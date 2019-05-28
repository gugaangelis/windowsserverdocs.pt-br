---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Adicionar inscrição\-na descrição de página
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 94ad9889ce78ba77f016210ee478a301babdf307
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190055"
---
# <a name="add-sign-in-page-description"></a>Adicionar inscrição\-na descrição de página


## <a name="to-add-sign-in-page-description"></a>Para adicionar entrada\-na descrição de página  
Para adicionar um sinal\-na descrição de página para a entrada\-na página, use o seguinte cmdlet do Windows PowerShell e a sintaxe.  

![Adicionar entrada na descrição](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> A cadeia de caracteres para o parâmetro `SignInPageDescriptionText` dá suporte a HTML puro com e sem tags. Portanto, você também pode executar o seguinte cmdlet sem usar o &lt;p&gt; marca.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Após o sinal\-na página é personalizado, a personalização terá prioridade; portanto, você deve personalizar para todos os idiomas que você deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Quando você configura o conteúdo localizado, ele deve ser configurado com um país\-menos localidade primeiro, por exemplo, "en", antes de configurar o país e região\-localidade específica, como "en\-us".  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
