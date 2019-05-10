---
title: Configurar os navegadores para usar a autenticação integrada do Windows (WIA) com o AD FS
description: Este documento descreve como configurar os navegadores para usar WIA com o AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f71680bb721635bd37197dca9d3ae4726099525f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845477"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurar os navegadores para usar a autenticação integrada do Windows (WIA) com o AD FS

Por padrão, a autenticação integrada do Windows (WIA) está habilitada no Active Directory Federation Services (AD FS) no Windows Server 2012 R2 para solicitações de autenticação que ocorrem na rede interna da organização (intranet) para qualquer aplicativo que usa um navegador para a sua autenticação.

AD FS 2016 agora tem uma configuração padrão aprimorada que permite que o navegador Microsoft Edge fazer WIA enquanto o Windows Phone não também (incorretamente) a captura também:

    =~Windows\s*NT.*Edge

Significa que você não precisa configurar cadeias de caracteres de agente de usuário individuais para dar suporte a cenários comuns de borda, mesmo que eles são atualizados com muita frequência.

Para outros navegadores, configure a propriedade do AD FS **WiaSupportedUserAgents** para adicionar os valores necessários com base nos navegadores que você está usando.  Você pode usar os procedimentos a seguir.



### <a name="view-wiasupporteduseragent-settings"></a>Exibir configurações de WIASupportedUserAgent
O **WIASupportedUserAgents** define os agentes do usuário que oferecem suporte a WIA. O AD FS analisa a cadeia de caracteres de agente do usuário ao executar logons em um navegador ou o controle do navegador.

Você pode exibir as configurações atuais usando o PowerShell de exemplo a seguir:

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Suporte a WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Alterar as configurações de WIASupportedUserAgent
Por padrão, uma nova instalação do AD FS tem um conjunto de correspondências de cadeia de caracteres de agente de usuário criado. No entanto, elas podem ser desatualizadas com base nas alterações para navegadores e dispositivos. Particularmente, os dispositivos Windows têm usuário agente cadeias de caracteres semelhantes com pequenas variações nos tokens. O Windows PowerShell exemplo a seguir fornece diretrizes recomendadas para o conjunto atual de dispositivos que estão disponíveis no mercado hoje que dão suporte a WIA contínuo:

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

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
