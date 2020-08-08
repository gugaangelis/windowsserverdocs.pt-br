---
ms.assetid: 309d6358-777d-496a-856d-728246c7d9a1
title: Personalizar os nomes de exibição e as descrições de métodos de autenticação
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 679f60d1328795ef4f272f0159c6be5185428d59
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954262"
---
# <a name="customize-the-display-names-and-descriptions-for-authentication-methods"></a>Personalizar os nomes de exibição e as descrições de métodos de autenticação

Para personalizar as descrições e os nomes de exibição para os métodos de autenticação, você pode usar o cmdlet do PowerShell do `Set-AdfsAuthenticationProviderWebContent` .  Para usar este cmdlt, você deve primeiro obter o nome do método de autenticação que você deseja personalizar.  Isso pode ser feito por meio do `Get-AdfsGlobalAuthenticationPolicy`.  No exemplo abaixo, vemos que, em nossa página de entrada \- , o seguinte é exibido: "entrar usando um certificado X. 509".  Queremos simplificar isso para nossos usuários.

![Personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update1.PNG)

Portanto, primeiro obtemos o nome do método de autenticação e, em seguida, editamos o texto exibido.

```powershell
Get-AdfsGlobalAuthenticationPolicy

AdditionalAuthenticationProvider  : {}
DeviceAuthenticationEnabled   : False
PrimaryIntranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}
PrimaryExtranetAuthenticationProvider : {FormsAuthentication, CertificateAuthentication}
WindowsIntegratedFallbackEnabled  : True

Set-AdfsAuthenticationProviderWebContent -Name CertificateAuthentication -DisplayName "Sign in with a certificate"
 ```

![Personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update2.PNG)

Agora vemos que nossa mensagem de exibição foi alterada.

![Personalizar DisplayName](media/AD-FS-user-sign-in-customization/ADFS_Customize_Update3.PNG)

## <a name="additional-references"></a>Referências adicionais

[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)
