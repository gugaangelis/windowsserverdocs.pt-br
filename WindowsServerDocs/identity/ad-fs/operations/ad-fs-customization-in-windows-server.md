---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: Personalização do AD FS no Windows Server 2016
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e4d463f5f25fe85dc95d767c9c75b722e60b012
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852027"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>Personalização do AD FS no Windows Server 2016

>Aplica-se a: Windows Server 2016

Em resposta a comentários de organizações que usam o AD FS, adicionamos ferramentas adicionais para personalizar o usuário entrar na experiência de aplicativos individuais protegidos pelo AD FS.  
Além de especificar o conteúdo por aplicativo web, como texto de descrição e links, agora você pode especificar temas da web inteira por aplicativo.  Isso inclui o logotipo, ilustração, folhas de estilo ou um arquivo onload.js inteira.  
  
## <a name="global-settings"></a>Configurações globais    
Para configurações globais gerais você pode consultar [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) fornecidos com o AD FS no Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Pré-requisitos  
Os seguintes pré-requisitos são necessários antes de tentar os procedimentos descritos neste documento.  
  
-   O AD FS no Windows Server 2016 TP4 ou posterior  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurar partes confiáveis do AD FS  
Por terceira parte da web de entrada elementos e os temas podem ser configurados usando os exemplos do PowerShell abaixo:  
  
### <a name="customize-messages"></a>Personalizar as mensagens  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personalizar a imagem, o logotipo e o nome da empresa  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>Personalizar a página inteira  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>Temas personalizados e temas personalizados avançados  
  
Para temas personalizados, consulte [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx) e [Advanced Customization of AD FS Sign-in Pages.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Atribuição de temas da web personalizado por RP  
  
Para atribuir um tema personalizado por RP use o procedimento a seguir:  
  
1. Criar um novo tema como uma cópia para o tema global no AD FS padrão  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code> 2.  Exportar o tema de personalização <code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>  
3. Personalizar arquivos de tema (imagens, css, onload.js) - a em seu editor favorito ou substituir o arquivo 4. Importar arquivos personalizados do sistema de arquivos para o AD FS (objetivando o novo tema) <code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>  
5. Aplicar o tema novo e personalizado para a RP específico (ou da RP) <code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>Descoberta de realm inicial  
Para descoberta de realm inicial consulte personalização [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Página de senha atualizada  
Para obter informações sobre como personalizar a página de atualização de senha, consulte [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>IDs de personalização e alternativas  
Os usuários podem entrar serviços de Federação do Active Directory (AD FS)-habilitado aplicativos usando qualquer forma de identificador de usuário que é aceito pelos serviços de domínio Active Directory (AD DS). Isso inclui os nomes de entidade de usuário (UPNs) (johndoe@contoso.com) ou de domínio qualificado de nomes de conta do sam (contoso\johndoe ou contoso.com\johndoe).  Para obter mais informações sobre isso, consulte [ID de configuração de logon alternativo.](Configuring-Alternate-Login-ID.md)  
  
Além disso você queira personalizar a página de entrada do AD FS para dar aos usuários finais alguma dica sobre a ID de logon alternativo. Você pode fazer isso adicionando a descrição de página de logon personalizada para obter mais informações, consulte [Customizing the AD FS Sign-in Pages.](https://technet.microsoft.com/library/dn280950.aspx)   
  
Você também pode fazer isso ao personalizar a cadeia de caracteres "Entrar com conta organizacional" acima do campo de nome de usuário.  Para obter informações sobre isso, consulte [Advanced Customization of AD FS Sign-in Pages](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
