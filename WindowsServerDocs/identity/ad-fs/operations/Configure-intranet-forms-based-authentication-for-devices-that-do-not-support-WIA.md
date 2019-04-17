---
ms.assetid: d562ef46-f240-48be-bbd4-fd88fc6bbbdc
title: "Configurando a autenticação baseada em formulários de intranet para dispositivos que não dão suporte a WIA"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: c78dc702cbff8eab487c6c077dfe57b0f0663342
ms.sourcegitcommit: 5012c078b410f59261edf0cc917393467243a5c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/23/2018
---
# <a name="configuring-intranet-forms-based-authentication-for-devices-that-do-not-support-wia"></a>Configurando a autenticação baseada em formulários de intranet para dispositivos que não dão suporte a WIA

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2

Por padrão, autenticação integrada do Windows (WIA) está habilitado nos serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2 para solicitações de autenticação que ocorrem na rede interna da organização (intranet) para qualquer aplicativo que usa um navegador para de autenticação. Por exemplo, esses podem ser aplicativos baseados em navegador que usam WS-Federation ou SAML protocolos e aplicativos que usam o protocolo OAuth. WIA fornece aos usuários finais logon perfeita para os aplicativos sem precisar inserir manualmente suas credenciais. No entanto, alguns dispositivos e navegadores não são capazes de dar suporte ao WIA e consequentemente solicitações de autenticação desses dispositivos falhar. Além disso, a experiência em determinados navegadores que negociar NTLM não é desejável. A abordagem recomendada é fallback para autenticação baseada em formulários para esses dispositivos e navegadores.

AD FS no Windows Server 2012 R2 e no Windows Server 2016 fornece os administradores a capacidade de configurar a lista de agentes do usuário que dão suporte o fallback para autenticação baseada em formulários. O fallback se torna possível por duas configurações:


- O **WIASupportedUserAgentStrings** propriedade do `Set-ADFSProperties`cmdlet
- O **WindowsIntegratedFallbackEnabled** propriedade do `Set-AdfsGlobalAuthenticationPolicy`commmandlet

O **WIASupportedUserAgentStrings** define os agentes de usuário que dão suporte WIA. AD FS analisa a cadeia de caracteres de agente do usuário ao executar logins em um navegador ou controle de navegador. Se o componente da cadeia de caracteres de agente do usuário não corresponde a nenhum dos componentes das cadeias de caracteres de agente de usuário que são configurados no **WIASupportedUserAgentStrings** propriedade, AD FS fará fallback para fornecer autenticação baseada em formulários, desde que o **WindowsIntegratedFallbackEnabled** sinalizador é definido como True.

Por padrão, uma nova instalação do AD FS tem um conjunto de correspondências de cadeia de caracteres de agente de usuário criadas. No entanto, esses podem ser desatualizados com base nas alterações de navegadores e dispositivos. Particularmente, dispositivos Windows têm caracteres de agente de usuário semelhante com pequenas variações nos tokens. O exemplo a seguir do Windows PowerShell oferece a melhor orientação para o conjunto de dispositivos que estão no mercado atualmente com suporte WIA perfeita atual:

    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")

O comando acima garante que o AD FS apenas aborda os seguintes casos de uso para WIA:

Agentes do usuário|Casos de uso|
-----|-----|
MSIE 6.0|O IE 6.0|
MSIE 7.0; Windows NT|IE 7, o IE na zona da intranet. O fragmento de "Windows NT" é enviado pelo sistema operacional de desktop.|
MSIE 8.0|IE 8.0 (dispositivos não enviá-lo, portanto, precisa fazer mais específicos)|
MSIE 9.0|IE 9.0 (dispositivos não enviá-lo, não precisa fazer isso mais específicos)|
MSIE 10.0; Windows NT 6|10.0 Do IE para Windows XP e versões mais recentes do sistema operacional de desktop</br></br>Dispositivos do Windows Phone 8.0 (com preferência definida para móvel) são excluídos porque eles enviar</br></br>Agente do usuário: Mozilla/5.0 (compatible; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Toque; NOKIA; Lumia 920)|
Windows NT 6.3; Trident/7.0</br></br>Windows NT 6.3; Win64; x64; Trident/7.0</br></br>Windows NT 6.3; WOW64; Trident/7.0| Windows 8.1 sistema operacional de desktop, diferentes plataformas|
Windows NT 6.2; Trident/7.0</br></br>Windows NT 6.2; Win64; x64; Trident/7.0</br></br>Windows NT 6.2; WOW64; Trident/7.0|Sistema de operacional da área de trabalho do Windows 8, diferentes plataformas|
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1; Win64; x64; Trident/7.0</br></br>Windows NT 6.1; WOW64; Trident/7.0|Sistema de operacional da área de trabalho do Windows 7, diferentes plataformas|
MSIPC| Proteção de informações da Microsoft e um cliente de controle|
Cliente de gerenciamento de direitos do Windows|Cliente de gerenciamento de direitos do Windows|

Para habilitar o fallback para a autenticação de formulário com base de agentes do usuário diferentes daquelas mencionado na cadeia de caracteres WIASupportedUserAgents, defina o sinalizador WindowsIntegratedFallbackEnabled como true

    Set-AdfsGlobalAuthenticationPolicy -WindowsIntegratedFallbackEnabled $true

Além disso, certifique-se de que a autenticação baseada em formulários é habilitada para intranet.

## <a name="configuring-wia-for-chrome"></a>Configurando WIA cromo
Você pode adicionar elementos visuais ou outros agentes do usuário à configuração do AD FS que dá suporte a WIA. Isso permite que o logon perfeita para aplicativos sem precisar inserir manualmente as credenciais quando você acessar recursos protegidos pelo AD FS. Siga as etapas abaixo para habilitar WIA no Chrome:

Adicionar uma cadeia de caracteres de agente do usuário para Chrome em configuração do AD FS

    Set-AdfsProperties -WIASupportedUserAgents ((Get-ADFSProperties | Select -ExpandProperty WIASupportedUserAgents) + “Chrome”)
    
Confirme se a cadeia de caracteres de agente do usuário para Chrome agora está definida nas propriedades do AD FS

    Get-AdfsProperties | Select -ExpandProperty WIASupportedUserAgents

![Configurar auth](media/Configure-intranet-forms-based-authentication-for-devices-that-do-not-support-WIA/chrome1.png) 

>[!NOTE]   
> Conforme novos navegadores e dispositivos são lançados, é recomendável que você reconcilia as funcionalidades desses agentes do usuário e atualizar a configuração do AD FS adequadamente para otimizar a experiência de autenticação do usuário quando usando disse navegador e dispositivos. Mais especificamente, é recomendável que você reavaliar o **WIASupportedUserAgents** configuração no AD FS ao adicionar um novo tipo de dispositivo ou navegador em sua matriz de suporte para WIA.


