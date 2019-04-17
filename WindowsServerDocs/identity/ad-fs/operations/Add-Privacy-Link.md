---
ms.assetid: 1ca6f87f-7272-4767-b609-3e295ac7d32f
title: Adicionar o Link de privacidade
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 81a453b45693b8222bdfc0231885b506fdfcd2fc
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="add-privacy-link"></a>Adicionar o Link de privacidade 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Para adicionar o link de privacidade que é exibido na página de sign\, use a sintaxe e o seguinte cmdlet do Windows PowerShell.  

![Adicionar o link de privacidade](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png) 
  
 
`Set-AdfsGlobalWebContent -PrivacyLink https://fs1.contoso.com/privacy/ -PrivacyLinkText Privacy`  
 
  
> [!IMPORTANT]  
> O `linkText`parâmetro neste cmdlet não é necessário, a menos que você usa outro valor que o padrão, que é *privacidade*. A vantagem de usar o padrão é que as páginas estão localizadas em todas as localidades de cliente. Depois que a página de sign\ é personalizada, a personalização tem precedência; Portanto, você deve personalizar para todos os idiomas que você deseja dar suporte. Todo o conteúdo personalizado leva um parâmetro de localidade. Quando você configura o conteúdo localizado, você deve configurá-la com uma localidade country\ menos pela primeira vez, por exemplo, "en", antes de você configurar país e da localidade region\ específicas, como "en\-us".  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
