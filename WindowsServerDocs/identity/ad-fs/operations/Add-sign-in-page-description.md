---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: Adicionar assinatura @ no__t-0in página descrição
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3b34a4e54aebd5b9dc3655eecd770a25f7ea97cf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407748"
---
# <a name="add-sign-in-page-description"></a>Adicionar assinatura @ no__t-0in página descrição


## <a name="to-add-sign-in-page-description"></a>Para adicionar o sinal @ no__t-0in página descrição  
Para adicionar uma descrição de página Sign @ no__t-0in à página Sign @ no__t-1in, use o seguinte cmdlet e sintaxe do Windows PowerShell.  

![Adicionar Descrição de entrada](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> A cadeia de caracteres para o parâmetro `SignInPageDescriptionText` dá suporte a HTML puro com e sem tags. Portanto, você também pode executar o cmdlet a seguir sem usar a marca &lt;p @ no__t-1.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Depois que a página Sign @ no__t-0in for personalizada, a personalização terá precedência; Portanto, você deve personalizar o para todos os idiomas aos quais deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Quando você configura conteúdo localizado, ele deve ser configurado com uma localidade @ no__t-0less primeiro, por exemplo, "en", antes de configurar o país e a região @ no__t-1specific localidade, como "en @ no__t-2US".  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
