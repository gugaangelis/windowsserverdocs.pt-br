---
title: Configurar navegadores para usar a autenticação integrada do Windows (WIA) com AD FS
description: Este documento descreve como configurar navegadores para usar o WIA com AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 03/20/2020
ms.topic: article
ms.openlocfilehash: e8a7b99ff18091266dd3ef7fdbe54ca641f4c556
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87956493"
---
# <a name="configure-browsers-to-use-windows-integrated-authentication-wia-with-ad-fs"></a>Configurar navegadores para usar a autenticação integrada do Windows (WIA) com AD FS

Por padrão, a autenticação integrada do Windows (WIA) é habilitada no Serviços de Federação do Active Directory (AD FS) (AD FS) no Windows Server 2012 R2 para solicitações de autenticação que ocorrem na rede interna da organização (intranet) para qualquer aplicativo que use um navegador para sua autenticação.

O AD FS 2016 agora tem uma configuração padrão aprimorada que permite que o navegador do Edge faça a WIA, embora não também (incorretamente) a captura de Windows Phone:

```
=~Windows\s*NT.*Edge
```

O acima significa que você não precisa mais configurar cadeias de caracteres individuais do agente do usuário para dar suporte a cenários comuns de borda, mesmo que elas sejam atualizadas com muita frequência.

Para outros navegadores, configure a propriedade AD FS **WiaSupportedUserAgents** para adicionar os valores necessários com base nos navegadores que você está usando.  Você pode usar os procedimentos a seguir.

### <a name="view-wiasupporteduseragent-settings"></a>Exibir configurações de WIASupportedUserAgent

O **WIASupportedUserAgents** define os agentes de usuário que dão suporte a WIA. AD FS analisa a cadeia de caracteres do agente do usuário ao executar logons em um navegador ou controle de navegador.

Você pode exibir as configurações atuais usando o seguinte exemplo do PowerShell:

```powershell
Get-AdfsProperties | select -ExpandProperty WiaSupportedUserAgents
```

![Suporte a WIA](../operations/media/Configure-AD-FS-Browser-WIA/wiasupport.png)

### <a name="change-wiasupporteduseragent-settings"></a>Alterar configurações de WIASupportedUserAgent
Por padrão, uma nova instalação do AD FS tem um conjunto de correspondências de cadeias de caracteres do agente do usuário criado. No entanto, elas podem estar desatualizadas com base nas alterações feitas em navegadores e dispositivos. Particularmente, os dispositivos Windows têm cadeias de caracteres de agente do usuário semelhantes com pequenas variações nos tokens. O exemplo do Windows PowerShell a seguir fornece a melhor orientação para o conjunto atual de dispositivos que estão no mercado atualmente que dão suporte a WIA contínuo:

Se você tiver AD FS no Windows Server 2012 R2 ou anterior:

```powershell
Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client", "Edg/79.0.309.43")
```

Se você tiver AD FS no Windows Server 2016 ou posterior:

```powershell
Set-AdfsProperties -WIASupportedUserAgents @("MSIE 6.0", "MSIE 7.0; Windows NT", "MSIE 8.0", "MSIE 9.0", "MSIE 10.0; Windows NT 6", "Windows NT 6.3; Trident/7.0", "Windows NT 6.3; Win64; x64; Trident/7.0", "Windows NT 6.3; WOW64; Trident/7.0", "Windows NT 6.2; Trident/7.0", "Windows NT 6.2; Win64; x64; Trident/7.0", "Windows NT 6.2; WOW64; Trident/7.0", "Windows NT 6.1; Trident/7.0", "Windows NT 6.1; Win64; x64; Trident/7.0", "Windows NT 6.1; WOW64; Trident/7.0", "MSIPC", "Windows Rights Management Client", "Edg/*")
```

O comando acima garantirá que AD FS abrange apenas os seguintes casos de uso para WIA:

|Agentes de usuário|Casos de uso|
|-----|-----|
|MSIE 6,0|IE 6,0|
|MSIE 7,0; Windows NT|IE 7, IE na zona da intranet. O fragmento "Windows NT" é enviado pelo sistema operacional da área de trabalho.|
|MSIE 8,0|IE 8,0 (nenhum dispositivo envia isso, portanto, precisa fazer mais específico)|
|MSIE 9,0|IE 9,0 (nenhum dispositivo envia isso, portanto, não é necessário tornar isso mais específico)|
|MSIE 10,0; Windows NT 6|IE 10,0 para Windows XP e versões mais recentes do sistema operacional de desktop</br></br>Windows Phone dispositivos 8,0 (com a preferência definida como móvel) são excluídos porque eles enviam</br></br>Agente do usuário: Mozilla/5.0 (compatível; MSIE 10,0; Windows Phone 8,0; Trident/6.0; IEMobile/10.0; BRAÇO Tom Nokia Lumia 920)|
|Windows NT 6,3; Trident/7.0</br></br>Windows NT 6,3; Win64 x86 Trident/7.0</br></br>Windows NT 6,3; WOW64 Trident/7.0| Windows 8.1 sistema operacional de desktop, diferentes plataformas|
|Windows NT 6,2; Trident/7.0</br></br>Windows NT 6,2; Win64 x86 Trident/7.0</br></br>Windows NT 6,2; WOW64 Trident/7.0|Sistema operacional Windows 8 desktop, diferentes plataformas|
|Windows NT 6,1; Trident/7.0</br></br>Windows NT 6,1; Win64 x86 Trident/7.0</br></br>Windows NT 6,1; WOW64 Trident/7.0|Sistema operacional Windows 7 Desktop, diferentes plataformas|
|Edg/79.0.309.43 | Microsoft Edge (Chromium) para Windows Server 2012 R2 ou anterior |
|Edg/*| Microsoft Edge (Chromium) para Windows Server 2016 ou posterior|
|MSIPC| Microsoft Information Protection and Control Client|
|Cliente do Windows Rights Management|Cliente do Windows Rights Management|

### <a name="additional-links"></a>Links adicionais

[Documentação do Microsoft Edge](/microsoft-edge/web-platform/user-agent-string)
