---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: Configurando a autenticação baseada em formulários de intranet para dispositivos que não dão suporte a WIA
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: cddc5d890114dec7e0053b16701db6f03c3cbbdf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889847"
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>Configurando a autenticação baseada em formulários de intranet para dispositivos que não dão suporte a WIA

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Por padrão, a autenticação integrada do Windows (WIA) está habilitada no Active Directory Federation Services (AD FS) no Windows Server 2012 R2 para solicitações de autenticação que ocorrem na rede interna da organização (intranet) para qualquer aplicativo que usa um navegador para a sua autenticação. Por exemplo, estes podem ser aplicativos baseados em navegador que usam o WS-Federation ou SAML protocolos e aplicativos sofisticados que usam o protocolo OAuth. WIA fornece aos usuários finais logon perfeita para os aplicativos sem precisar entrar manualmente suas credenciais. No entanto, alguns navegadores e dispositivos não são capazes de dar suporte a WIA e falharem como resultado de solicitações de autenticação desses dispositivos. Além disso, a experiência em determinados navegadores que negociam para NTLM não é desejável. A abordagem recomendada é fazer fallback para autenticação baseada em formulários para esses dispositivos e navegadores.

AD FS no Windows Server 2016 e Windows Server 2012 R2 fornece os administradores a capacidade de configurar a lista de agentes de usuário que suporta o fallback para a autenticação baseada em formulários. O fallback é possibilitado por duas configurações:


- O **WIASupportedUserAgentStrings** propriedade do `Set-ADFSProperties` commandlet
- O **WindowsIntegratedFallbackEnabled** propriedade do `Set-AdfsGlobalAuthenticationPolicy` commandlet

O **WIASupportedUserAgentStrings** define os agentes do usuário que oferecem suporte a WIA. O AD FS analisa a cadeia de caracteres de agente do usuário ao executar logons em um navegador ou o controle do navegador. Se o componente da cadeia de caracteres de agente do usuário não corresponde a nenhum dos componentes das cadeias de caracteres de agente de usuário que são configurados nas **WIASupportedUserAgentStrings** propriedade, o AD FS fará o fallback para fornecendo autenticação baseada em formulários desde que o **WindowsIntegratedFallbackEnabled** sinalizador é definido como True.

Por padrão, uma nova instalação do AD FS tem um conjunto de correspondências de cadeia de caracteres de agente de usuário criado. No entanto, elas podem ser desatualizadas com base nas alterações para navegadores e dispositivos. Particularmente, os dispositivos Windows têm usuário agente cadeias de caracteres semelhantes com pequenas variações nos tokens. O Windows PowerShell exemplo a seguir fornece diretrizes recomendadas para o conjunto atual de dispositivos que estão disponíveis no mercado hoje que dão suporte a WIA contínuo:

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

O comando acima garantirá que o AD FS aborda somente os seguintes casos de uso para WIA:

Agentes de usuário|Casos de uso|
-----|-----|
MSIE 6.0|IE 6.0|
MSIE 7.0; Windows NT|O IE 7, IE na zona da intranet. O fragmento de "Windows NT" é enviado pelo sistema operacional da área de trabalho.|
MSIE 8.0|Internet Explorer 8.0 (nenhum dispositivo enviá-lo, portanto, precisa fazer mais específico)|
MSIE 9.0|9.0 do IE (não há dispositivos enviá-lo, para que você não precisa fazer isso mais específico)|
MSIE 10.0; Windows NT 6|10.0 do IE para Windows XP e versões mais recentes do sistema operacional de desktop</br></br>Dispositivos Windows Phone 8.0 (com preferência definida para móvel) serão excluídos porque eles enviam</br></br>Agente do usuário: Mozilla/5.0 (compatível; MSIE 10.0; Windows Phone 8.0; Trident/6.0; Iemobile que está/10.0; ARM; Toque; NOKIA; Lumia 920)|
Windows NT 6.3; Trident/7.0</br></br>Windows NT 6.3; Win64; x64; Trident/7.0</br></br>Windows NT 6.3; WOW64; Trident/7.0| Sistema de operacional da área de trabalho do Windows 8.1, plataformas diferentes|
Windows NT 6.2; Trident/7.0</br></br>Windows NT 6.2; Win64; x64; Trident/7.0</br></br>Windows NT 6.2; WOW64; Trident/7.0|Sistema de operacional da área de trabalho do Windows 8, plataformas diferentes|
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1; Win64; x64; Trident/7.0</br></br>Windows NT 6.1; WOW64; Trident/7.0|Sistema de operacional da área de trabalho do Windows 7, plataformas diferentes|
MSIPC| Cliente de controle e proteção de informações da Microsoft|
Cliente do Windows Rights Management|Cliente do Windows Rights Management|

Para permitir fallback para autenticação de formulário com base para os agentes de usuário além daqueles mencionados na cadeia de caracteres WIASupportedUserAgents, defina o sinalizador de WindowsIntegratedFallbackEnabled como true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

Certifique-se também de que a autenticação baseada em formulários está habilitada para a intranet.

## <a name="configuring-wia-for-chrome"></a>Configurar WIA para Chrome
Você pode adicionar o Chrome ou outros agentes de usuário para a configuração do AD FS que dá suporte a WIA. Isso permite que o logon perfeita para aplicativos sem precisar inserir manualmente as credenciais ao acessar recursos protegidos pelo AD FS. Siga as etapas abaixo para habilitar a WIA no Chrome:

Adicionar uma cadeia de caracteres de agente do usuário para o Chrome na configuração do AD FS

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
Confirme se a cadeia de caracteres de agente do usuário para o Chrome agora está definida nas propriedades do AD FS

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![Configurar autenticação](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> Quando novos navegadores e dispositivos são lançados, é recomendável que você reconcilie as funcionalidades desses agentes de usuário e atualizar a configuração do AD FS adequadamente para otimizar a experiência de autenticação do usuário quando usando disse que o navegador e dispositivos. Mais especificamente, é recomendável que você reavalie as **WIASupportedUserAgents** configuração no AD FS ao adicionar um novo tipo de dispositivo ou navegador WIA sua matriz de suporte.


