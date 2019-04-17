---
title: "Configurar navegadores para usar autenticação integrada do Windows (WIA) com o AD FS"
description: Este documento descreve como configurar os navegadores para usar WIA com o AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f7d43931d1fe4958a539ff1b728e4cc154d06248
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurar navegadores para usar autenticação integrada do Windows (WIA) com o AD FS

Por padrão, autenticação integrada do Windows (WIA) está habilitado nos serviços de Federação do Active Directory (AD FS) no Windows Server 2012 R2 para solicitações de autenticação que ocorrem na rede interna da organização (intranet) para qualquer aplicativo que usa um navegador para de autenticação.

AD FS 2016 agora tem uma configuração padrão aprimorado que permite que o navegador Edge fazer WIA enquanto o Windows Phone não também (incorretamente) inoperantes também:

    =~Windows\s*NT.*Edge

A acima significa que você não precisa configurar cadeias de caracteres de agente de usuário individual para dar suporte a cenários comuns de borda, mesmo que eles estejam atualizados muito frequentemente.

Para outros navegadores, configurar a propriedade AD FS **WiaSupportedUserAgents** para adicionar os valores necessários com base nos navegadores que você está usando.  Você pode usar os procedimentos a seguir.



### <a name="view-wiasupporteduseragent-settings"></a>Configurações de exibição de WIASupportedUserAgent
O **WIASupportedUserAgents** define os agentes de usuário que dão suporte WIA. AD FS analisa a cadeia de caracteres de agente do usuário ao executar logins em um navegador ou controle de navegador.

Você pode exibir as configurações atuais usando o PowerShell de exemplo a seguir:

```powershell
    $strings = Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Suporte WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Alterar as configurações de WIASupportedUserAgent
Por padrão, uma nova instalação do AD FS tem um conjunto de correspondências de cadeia de caracteres de agente de usuário criadas. No entanto, esses podem ser desatualizados com base nas alterações de navegadores e dispositivos. Particularmente, dispositivos Windows têm caracteres de agente de usuário semelhante com pequenas variações nos tokens. O exemplo a seguir do Windows PowerShell oferece a melhor orientação para o conjunto de dispositivos que estão no mercado atualmente com suporte WIA perfeita atual:

```powershell
    Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client")
```

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
Windows NT 6.1; Trident/7.0</br></br>Windows NT 6.1; Win64; x64; Trident/7.0</br></br>Windows NT 6.1; WOW64; Trident/7.0|Sistema de operacional da área de trabalho do Windows 7, platoforms diferentes|
MSIPC| Proteção de informações da Microsoft e um cliente de controle|
Cliente de gerenciamento de direitos do Windows|Cliente de gerenciamento de direitos do Windows|
