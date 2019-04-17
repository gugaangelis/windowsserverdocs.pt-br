---
ms.assetid: 330c7b61-dde0-432f-9b74-d250ad9cc808
title: "Adicionar sign\\ na página de descrição"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 128a4ffd8d4b9dfcfe5f0e8e2df8a0e1505cbb24
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="add-sign-in-page-description"></a>Adicionar sign\ na página de descrição

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

## <a name="to-add-sign-in-page-description"></a>Para adicionar sign\ na página de descrição  
Para adicionar uma descrição sign\ na página para a página de sign\, use a sintaxe e o seguinte cmdlet do PowerShell do Windows PowerShell.  

![Adicionar log na descrição](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom2.png)

    Set-AdfsGlobalWebContent -SignInPageDescriptionText "<p>Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information.</p>" 
 
  
> [!IMPORTANT]  
> A cadeia de caracteres para o `SignInPageDescriptionText`parâmetro dá suporte a ambas as HTML puro com as marcas e sem. Portanto, você também pode executar o seguinte cmdlet sem usar o &lt;p&gt; marca.  `Set-AdfsGlobalWebContent -SignInPageDescriptionText "Sign-in to Contoso requires device registration. Click <A href='http://fs1.contoso.com/deviceregistration/'>here</A> for more information." ` 

Depois que a página de sign\ é personalizada, a personalização tem precedência; Portanto, você deve personalizar para todos os idiomas que você deseja dar suporte. Todo o conteúdo personalizado leva um parâmetro de localidade. Quando você configura o conteúdo localizado, ele deverá ser configurado com uma localidade country\ menos pela primeira vez, por exemplo, "en", antes de você configurar país e da localidade region\ específicas, como "en\-us".  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
