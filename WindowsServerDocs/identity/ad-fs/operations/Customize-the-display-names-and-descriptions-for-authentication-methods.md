---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: "Personalizar os nomes de exibição e as descrições para métodos de autenticação"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 699622a8a075dd6c78ab1b536dce2abfee642e9e
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizar os nomes de exibição e as descrições para métodos de autenticação 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Para personalizar os nomes de exibição e as descrições para métodos de autenticação, você pode usar o `Set-AdfsAuthenticationProviderWebContent`cmdlt do PowerShell.  Para usar este cmdlt, primeiro você deve obter o nome do método de autenticação que você deseja personalizar.  Isso pode ser feito usando `Get-AdfsGlobalAuthenticationPolicy`.  No exemplo a seguir vemos que, na página nossa sign\, o seguinte é exibido: "logon usando um certificado x. 509".  Queremos simplificar essa ação para nossos usuários.  
  
![Personalizar displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
Primeiro, temos o nome do método de autenticação e, em seguida, edite o texto exibido.  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![Personalizar displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
Agora, vemos que nosso Exibir mensagem foi alterada.  
  
![Personalizar displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md) 
