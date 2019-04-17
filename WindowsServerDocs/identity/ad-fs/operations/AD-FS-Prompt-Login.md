---
title: Prompt do AD FS = logon
description: Perguntas frequentes para o AD FS 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/27/2017
ms.topic: article
ms.custom: it-pro
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 8b98cdc27c9bd01d9855e98965f59ebe5257ed5c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Solicitar que os serviços de Federação do Active Directory = suporte de parâmetro de logon
Este documento descreve o suporte nativo para o parâmetro de logon = prompt que está disponível no AD FS.

## <a name="what-is-promptlogin"></a>O que é prompt = logon?  

Alguns aplicativos do Office 365 (com autenticação moderno habilitado) enviam o parâmetro de logon = prompt ao Azure AD como parte de cada solicitação de autenticação.  Por padrão, Azure AD transforma isso em dois parâmetros: <code><b>wauth</b>=urn:oasis:names:tc:SAML:1.0:am:password</code>, e <code><b>wfresh</b>=0</code>.

Isso pode causar problemas de intranet corporativa e cenários de autenticação multifator no qual um tipo de autenticação que não seja o nome de usuário e senha é desejado.  

AD FS no Windows Server 2012 R2 com o pacote cumulativo de julho de 2016 introduziu suporte nativo para o parâmetro = prompt de logon.  Isso significa que, agora você tem a opção de configurar o Azure AD para enviar esse parâmetro como-é seu serviço AD FS como parte do Azure AD e solicitações de autenticação do Office 365.

### <a name="ad-fs-versions-that-support-promptlogin"></a>As versões do AD FS que dão suporte a prompt = logon
A seguir está uma lista das versões do AD FS que dá suporte ao parâmetro de logon = prompt.

- Pacote cumulativo do AD FS no Windows Server 2012 R2 com julho de 2016

- AD FS no Windows Server 2016

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>Como fazer para configurar seu locatário do Azure AD para enviar solicitação = efetuar login no AD FS

Use o módulo do PowerShell do Azure AD para definir a configuração.

> [!NOTE]
> A funcionalidade de logon = prompt (habilitada pela propriedade PromptLoginBehavior) está disponível apenas no momento o [' versão 1.0' Azure AD módulo do Powershell](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), em que os cmdlets têm nomes que incluem "Msol", como Set-MsolDomainFederationSettings.  Não é atualmente disponível através do ' versão 2.0' Azure AD módulo do PowerShell, cujos cmdlets têm nomes como "definir-AzureAD\ *".

Para configurar o prompt = comportamento de logon, a sintaxe do cmdlet abaixo:

Exemplo 1:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PreferredAuthenticationProtocol <your current protocol setting> 
```

Exemplo 2:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -SupportsMfa <$True|$False>
```

Exemplo 3:
```powershell
    Set-MsolDomainFederationSettings –DomainName <your domain name> -PromptLoginBehavior <TranslateToFreshPasswordAuth|NativeSupport|Disabled>
```

 
 Os valores de propriedade PreferredAuthenticationProtocol, SupportsMfa e PromptLoginBehavior podem ser encontrados observando a saída do cmdlet: ![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> Por padrão, quando executado Get-MsolDomainFederationSettings, determinadas propriedades não são exibidas no console.  Para exibir esses parâmetros, é recomendável que você use o | Flórida * para forçar a saída de todas as propriedades do objeto.


A seguir há mais informações sobre o parâmetro PromptLoginBehavior e suas configurações.
   
   - <b>TranslateToFreshPasswordAuth</b> significa que o comportamento padrão do Azure AD enviando <b>wauth</b> e <b>wfresh</b> para AD FS em vez de prompt = logon
   - <b>NativeSupport</b> significa que o parâmetro de logon = solicitação será enviado como está para o AD FS
   - <b>Desabilitado</b> significa nada será enviado para o AD FS

