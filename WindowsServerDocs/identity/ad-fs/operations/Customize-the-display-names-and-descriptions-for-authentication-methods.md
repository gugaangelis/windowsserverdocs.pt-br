---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: Personalizar os nomes de exibição e as descrições de métodos de autenticação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 218643bbd5ada63b6bee2b91a7bace3f9959c0bd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80816439"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizar os nomes de exibição e as descrições de métodos de autenticação 


Para personalizar as descrições e os nomes de exibição para os métodos de autenticação, você pode usar o cmdlet do PowerShell do `Set-AdfsAuthenticationProviderWebContent` .  Para usar este cmdlt, você deve primeiro obter o nome do método de autenticação que você deseja personalizar.  Isso pode ser feito por meio do `Get-AdfsGlobalAuthenticationPolicy`.  No exemplo abaixo, vemos que, em nosso\-de entrada na página, o seguinte é exibido: "entrar usando um certificado X. 509".  Queremos simplificar isso para nossos usuários.  
  
![Personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)  
  
Portanto, primeiro obtemos o nome do método de autenticação e, em seguida, editamos o texto exibido.  
  
 
    Get-AdfsGlobalAuthenticationPolicy  
      
    AdditionalAuthenticationProvider  : {}  
    DeviceAuthenticationEnabled   : False  
    PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}  
    WindowsIntegratedFallbackEnabled  : True  
      
    Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"  
  
  
![Personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)  
  
Agora vemos que nossa mensagem de exibição foi alterada.  
  
![Personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md) 
