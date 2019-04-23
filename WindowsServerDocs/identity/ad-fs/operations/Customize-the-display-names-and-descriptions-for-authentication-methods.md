---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: Personalizar os nomes de exibição e descrições para métodos de autenticação
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 699622a8a075dd6c78ab1b536dce2abfee642e9e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855187"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizar os nomes de exibição e descrições para métodos de autenticação 

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Para personalizar as descrições e os nomes de exibição para os métodos de autenticação, você pode usar o cmdlet do PowerShell do `Set-AdfsAuthenticationProviderWebContent` .  Para usar este cmdlt, você deve primeiro obter o nome do método de autenticação que você deseja personalizar.  Isso pode ser feito por meio do `Get-AdfsGlobalAuthenticationPolicy`.  No exemplo a seguir vemos que, em nossa entrada\-na página, o seguinte é exibido:  "Entre usando um certificado X.509".  Queremos simplificar isso para nossos usuários.  
  
![Personalizar o displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
Portanto, primeiro obtemos o nome do método de autenticação e, em seguida, editamos o texto exibido.  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![Personalizar o displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
Agora vemos que nossa mensagem de exibição foi alterada.  
  
![Personalizar o displayname](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md) 
