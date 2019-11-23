---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Adicione link de privacidade
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cd559e38c38e96d1417257fe7d6ff8ccfa180c6b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358424"
---
# <a name="add-privacy-link"></a>Adicione link de privacidade 


Para adicionar o link de privacidade exibido na página\-de entrada, use o cmdlet e a sintaxe do Windows PowerShell a seguir.  

![Adicionar link de privacidade](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> O parâmetro `linkText` nesse cmdlet não é necessário, a menos que você use outro valor diferente do padrão, que é *Privacidade*. A vantagem de usar o padrão é que as páginas são localizadas para todas as localidades de cliente. Depois que o sinal\-na página é personalizado, a personalização tem precedência; Portanto, você deve personalizar o para todos os idiomas aos quais deseja dar suporte. Todo conteúdo personalizado possui um parâmetro de localidade. Ao configurar conteúdo localizado, você deve configurá-lo com um país\-menos localidade, por exemplo, "en", antes de configurar país e região\-localidade específica, como "en\-US".  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
