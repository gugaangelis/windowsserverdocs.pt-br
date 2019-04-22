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
ms.openlocfilehash: 6a4b6cfe98064181824e210be9031a0f67cb4b75
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824627"
---
# <a name="active-directory-federation-services-promptlogin-parameter-support"></a>Serviços de Federação do Active Directory solicitará = suporte ao parâmetro de logon
Este documento descreve o suporte nativo para o parâmetro de prompt = logon que está disponível no AD FS.

## <a name="what-is-promptlogin"></a>O que é o prompt = login?  

Alguns aplicativos do Office 365 (com autenticação moderna habilitada) enviam o parâmetro de logon = prompt para o Azure AD como parte de cada solicitação de autenticação.  Por padrão, Azure AD converte isso em dois parâmetros: <code> <b> wauth </b> =urn:oasis:names:tc:SAML:1.0:am:password </code>, e <code> <b> wfresh </b> =0 </code> .

Isso pode causar problemas com a intranet corporativa e cenários de autenticação multifator em que um tipo de autenticação que não seja o nome de usuário e senha é desejado.  

AD FS no Windows Server 2012 R2 com pacote cumulativo de atualizações de julho de 2016 introduziu o suporte nativo para o parâmetro do prompt = login.  Isso significa que, agora você tem a opção de configuração do Azure AD para enviar este parâmetro como-está ao seu serviço do AD FS como parte do AD do Azure e as solicitações de autenticação do Office 365.

### <a name="ad-fs-versions-that-support-promptlogin"></a>Versões de FS AD que dão suporte ao prompt = login
O exemplo a seguir é uma lista das versões do AD FS que dão suporte ao parâmetro prompt = login.

- Atualização cumulativa de AD FS no Windows Server 2012 R2 com a julho de 2016

- O AD FS no Windows Server 2016

## <a name="how-do-to-configure-your-azure-ad-tenant-to-send-promptlogin-to-ad-fs"></a>Como fazer para configurar seu locatário do AD do Azure para enviar um prompt = login para o AD FS

Use o módulo do PowerShell do Azure AD para definir a configuração.

> [!NOTE]
> A capacidade de logon de prompt = (habilitada pela propriedade PromptLoginBehavior) está atualmente disponível apenas na [' módulo do Azure AD Powershell versão 1.0"](https://connect.microsoft.com/site1164/Downloads/DownloadDetails.aspx?DownloadID=59185), no qual os cmdlets têm nomes que incluam"Msol", como Set-MsolDomainFederationSettings.  Ele não está disponível atualmente por meio de ' versão 2.0' módulo do Azure AD PowerShell, cujos cmdlets têm nomes como "Set-AzureAD\*".

Para configurar o prompt = o comportamento de logon, a sintaxe do cmdlet abaixo:

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

 
 Os valores de propriedade PreferredAuthenticationProtocol, SupportsMfa e PromptLoginBehavior podem ser encontrados visualizando a saída do cmdlet: ![Get-MsolDomainFederationSettings](media/AD-FS-Prompt-Login/GetMsol.png)
```powershell
    Get-MsolDomainFederationSettings -DomainName <your_domain_name> | fl *
 ```
> [!NOTE]
> Por padrão, ao executar Get-MsolDomainFederationSettings, determinadas propriedades não são exibidas no console.  Para exibir esses parâmetros, é recomendável que você use o | FL * para forçar a saída de todas as propriedades do objeto.


O seguinte é obter mais informações sobre o parâmetro PromptLoginBehavior e suas configurações.
   
   - <b>TranslateToFreshPasswordAuth</b> significa que o comportamento do AD do Azure padrão de envio <b>wauth</b> e <b>wfresh</b> para o AD FS, em vez de prompt = login
   - <b>NativeSupport</b> significa que o parâmetro do prompt = login será enviado como está para o AD FS
   - <b>Desabilitado</b> significa que nada será enviado para o AD FS

