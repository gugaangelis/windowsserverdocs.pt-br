---
ms.assetid: 1df78c2a-5054-4b54-8310-c48ea62e6e0b
title: "Mensagens de erro personalizadas para a página de entrada do AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 02/09/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b73caad520abae1fcfdbd5d04b88c0608df12cca
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="custom-error-messages-for-ad-fs-sign-in-page"></a>Mensagens de erro personalizadas para a página de entrada do AD FS  

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Você pode configurar mensagens de erro personalizadas que podem ser adaptadas para sua organização. A ilustração a seguir mostra uma descrição da página de erro personalizada e uma mensagem de erro genérica. Use os seguintes cmdlets do Windows PowerShell para personalizar suas mensagens de erro.  
  
![Erro personalizadas](media/AD-FS-user-sign-in-customization/ADFS_Blue_Custom3.png)  
  
## <a name="customize-the-error-page-description"></a>Personalizar a descrição da página de erro  
Para personalizar a descrição da página de erro, use a sintaxe e o seguinte cmdlet do Windows PowerShell.  
  

`Set-AdfsGlobalWebContent -ErrorPageDescriptionText "This is Contoso's error page description" ` 

  
## <a name="customize-a-generic-error-message"></a>Personalizar uma mensagem de erro genérica  
Para personalizar a mensagem de erro genérica, use a sintaxe e o seguinte cmdlet do Windows PowerShell.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageGenericErrorMessage "This is a generic error message.  Contact Contoso IT for assistance." ` 

  
## <a name="customize-an-authorization-error-message"></a>Personalizar uma mensagem de erro de autorização  
Para personalizar a mensagem de erro de autorização, use a sintaxe e o seguinte cmdlet do Windows PowerShell.  
  

    Set-AdfsGlobalWebContent -ErrorPageAuthorizationErrorMessage "You have received an Authorization error.  Contact Contoso IT for assistance."  

  
## <a name="customize-a-device-authentication-error-message"></a>Personalizar a mensagem de erro de autenticação de dispositivo  
Para personalizar a mensagem de erro de autenticação de dispositivo, use a sintaxe e o seguinte cmdlet do Windows PowerShell.  
  
 
`Set-AdfsGlobalWebContent -ErrorPageDeviceAuthenticationErrorMessage "Your device is not authorized.  Contact Contoso IT for assistance."`  
 
  
## <a name="customize-a-support-email-error-message"></a>Personalizar uma mensagem de erro de email de suporte  
Você pode configurar um endereço de email de suporte no AD FS. Se configurado, o AD FS mostra automaticamente um link para os usuários finais para enviar por email os detalhes do erro.  
  
Para personalizar a mensagem de erro de email de suporte, use a sintaxe e o seguinte cmdlet do Windows PowerShell.  
  

    Set-AdfsGlobalWebContent -ErrorPageSupportEmail  "admin@contoso.com"  

  
## <a name="customize-a-relying-party-authorization-message"></a>Personalizar uma terceira mensagem de autorização de terceiros  
Você pode configurar uma terceira mensagem de erro de autorização de terceiros no AD FS.  
  
Para personalizar a mensagem de erro de terceiros confiante, use a sintaxe e o seguinte cmdlet do Windows PowerShell.  

    Set-AdfsRelyingPartyWebContent -Name fedpassive -ErrorPageAuthorizationErrorMessage "<p> You need to be a member of Security Auditors to access this site. Click <A href='http://accessrequest/'>here</A> for more information.</p>“  


## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)    
