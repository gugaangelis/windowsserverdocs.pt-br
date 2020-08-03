---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: Configurando a autenticação baseada em formulários de intranet para dispositivos que não dão suporte a WIA
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 781f950041666ba184fc522a55cbf23a54e6dd08
ms.sourcegitcommit: 3632b72f63fe4e70eea6c2e97f17d54cb49566fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2020
ms.locfileid: "87519775"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>Configurando a autenticação baseada em formulários de intranet para dispositivos que não dão suporte a WIA

Por padrão, a autenticação integrada do Windows (WIA) é habilitada no Serviços de Federação do Active Directory (AD FS) (AD FS) no Windows Server 2012 R2 para solicitações de autenticação que ocorrem na rede interna da organização (intranet) para qualquer aplicativo que use um navegador para sua autenticação. Por exemplo, eles podem ser aplicativos baseados em navegador que usam protocolos WS-Federation ou SAML e aplicativos avançados que usam o protocolo OAuth. O WIA fornece aos usuários finais um logon contínuo para os aplicativos sem precisar inserir manualmente suas credenciais. No entanto, alguns dispositivos e navegadores não são capazes de dar suporte a WIA e, como resultado, solicitações de autenticação desses dispositivos falham. Além disso, a experiência em determinados navegadores que negociam com o NTLM não é desejável. A abordagem recomendada é fazer fallback para a autenticação baseada em formulários para esses dispositivos e navegadores.

AD FS no Windows Server 2016 e no Windows Server 2012 R2 fornece aos administradores a capacidade de configurar a lista de agentes de usuário que dão suporte ao fallback para autenticação baseada em formulários. O fallback se torna possível por duas configurações:

- A propriedade **WIASupportedUserAgentStrings** do `Set-ADFSProperties` commandlet
- A propriedade **WindowsIntegratedFallbackEnabled** do `Set-AdfsGlobalAuthenticationPolicy` commandlet

O **WIASupportedUserAgentStrings** define os agentes de usuário que dão suporte a WIA. AD FS analisa a cadeia de caracteres do agente do usuário ao executar logons em um navegador ou controle de navegador. Se o componente da cadeia de caracteres do agente do usuário não corresponder a nenhum dos componentes das cadeias de caracteres do agente do usuário configurados na propriedade **WIASupportedUserAgentStrings** , o AD FS voltará a fornecer autenticação baseada em formulários, desde que o sinalizador **WindowsIntegratedFallbackEnabled** seja definido como true.

Por padrão, uma nova instalação do AD FS tem um conjunto de correspondências de cadeias de caracteres do agente do usuário criado. No entanto, elas podem estar desatualizadas com base nas alterações feitas em navegadores e dispositivos. Particularmente, os dispositivos Windows têm cadeias de caracteres de agente do usuário semelhantes com pequenas variações nos tokens. O exemplo do Windows PowerShell a seguir fornece a melhor orientação para o conjunto atual de dispositivos que estão no mercado atualmente que dão suporte a WIA contínuo:

```powershell
Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

O comando acima garantirá que AD FS abrange apenas os seguintes casos de uso para WIA:

Agentes de usuário|Casos de uso|
-----|-----|
MSIE 6,0|IE 6,0|
MSIE 7,0; Windows NT|IE 7, IE na zona da intranet. O fragmento "Windows NT" é enviado pelo sistema operacional da área de trabalho.|
MSIE 8,0|IE 8,0 (nenhum dispositivo envia isso, portanto, precisa fazer mais específico)|
MSIE 9,0|IE 9,0 (nenhum dispositivo envia isso, portanto, não é necessário tornar isso mais específico)|
MSIE 10,0; Windows NT 6|IE 10,0 para Windows XP e versões mais recentes do sistema operacional de desktop</br></br>Windows Phone dispositivos 8,0 (com a preferência definida como móvel) são excluídos porque eles enviam</br></br>Agente do usuário: Mozilla/5.0 (compatível; MSIE 10,0; Windows Phone 8,0; Trident/6.0; IEMobile/10.0; BRAÇO Tom Nokia Lumia 920)|
Windows NT 6,3; Trident/7.0</br></br>Windows NT 6,3; Win64 x86 Trident/7.0</br></br>Windows NT 6,3; WOW64 Trident/7.0| Windows 8.1 sistema operacional de desktop, diferentes plataformas|
Windows NT 6,2; Trident/7.0</br></br>Windows NT 6,2; Win64 x86 Trident/7.0</br></br>Windows NT 6,2; WOW64 Trident/7.0|Sistema operacional Windows 8 desktop, diferentes plataformas|
Windows NT 6,1; Trident/7.0</br></br>Windows NT 6,1; Win64 x86 Trident/7.0</br></br>Windows NT 6,1; WOW64 Trident/7.0|Sistema operacional Windows 7 Desktop, diferentes plataformas|
MSIPC| Microsoft Information Protection and Control Client|
Cliente do Windows Rights Management|Cliente do Windows Rights Management|

Para habilitar o fallback para autenticação baseada em formulário para agentes de usuário diferentes daqueles mencionados na cadeia de caracteres WIASupportedUserAgents, defina o sinalizador WindowsIntegratedFallbackEnabled como true

```powershell
Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true
```

Verifique também se a autenticação baseada em formulários está habilitada para intranet.

## <a name="configuring-wia-for-chrome"></a>Configurando o WIA para Chrome
Você pode adicionar outros agentes do Chrome ou de outros usuários à configuração de AD FS que dá suporte a WIA. Isso permite fazer logon contínuo em aplicativos sem precisar inserir manualmente as credenciais ao acessar os recursos protegidos pelo AD FS. Siga as etapas abaixo para habilitar o WIA no Chrome:

Na configuração do AD FS, adicione uma cadeia de caracteres do agente do usuário para o Chrome em plataformas baseadas no Windows:

```powershell
Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Windows NT)")
```

E, da mesma forma, para o Chrome no Apple macOS, adicione a seguinte cadeia de caracteres do agente do usuário à configuração do AD FS:

```powershell
Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + "Mozilla/5.0 (Macintosh; Intel Mac OS X)")
```

Confirme se a cadeia de caracteres do agente do usuário para Chrome agora está definida nas propriedades AD FS:

```powershell
Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents
```

(Você precisaria de uma nova captura de tela aqui) ![ configurar autenticação](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png)

>[!NOTE]
> À medida que novos dispositivos e navegadores são liberados, é recomendável que você reconcilie os recursos desses agentes de usuário e atualize a configuração de AD FS apropriadamente para otimizar a experiência de autenticação do usuário ao usar o navegador e os dispositivos. Mais especificamente, é recomendável que você reavalie a configuração **WIASupportedUserAgents** em AD FS ao adicionar um novo tipo de dispositivo ou navegador à sua matriz de suporte para WIA.
