---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: "AD FS personalização no Windows Server 2016"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2e4d463f5f25fe85dc95d767c9c75b722e60b012
ms.sourcegitcommit: a2699e93a0a19cb138c1fde0c9af36774a12f865
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2017
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>AD FS personalização no Windows Server 2016

>Aplica-se a: Windows Server 2016

Em resposta aos comentários das organizações usando o AD FS, adicionamos ferramentas adicionais para personalizar o usuário faça logon na experiência para aplicativos individuais protegido pelo AD FS.  
Além de especificar o conteúdo de cada aplicativo web, como texto de descrição e links, agora você pode especificar temas web inteira por aplicativo.  Isso inclui o logotipo, ilustração, folhas de estilos ou um arquivo todo onload.js.  
  
## <a name="global-settings"></a>Configurações globais    
Para configurações globais gerais você pode consultar [Personalizando as páginas do AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx) fornecidos com o AD FS no Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Pré-requisitos  
Pré-requisitos a seguir são necessárias antes de tentar os procedimentos descritos neste documento.  
  
-   AD FS no Windows Server 2016 TP4 ou posterior  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurar partes confiáveis do AD FS  
Por terceira parte entrar web elementos e temas podem ser configurados usando o PowerShell os exemplos a seguir:  
  
### <a name="customize-messages"></a>Personalizar mensagens  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personalizar a imagem, logotipo e nome da empresa  
  
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
  
Para temas personalizados, consulte [Personalizando as páginas do AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx) e [avançada personalização das páginas do AD FS Sign-in.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Atribuindo temas web personalizada por RP  
  
Para atribuir um tema personalizado por RP use o procedimento a seguir:  
  
1. Criar um novo tema como uma cópia para o padrão, o tema global do AD FS  
<code>New-AdfsWebTheme -Name AppSpecificTheme -SourceName default</code>  
2.  Exportar o tema de personalização <code>Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme</code>3.personalizar arquivos de tema (imagens, css, onload.js) em seu editor favorito ou substituir o arquivo de 4. Importar arquivos personalizados do sistema de arquivos para o AD FS (direcionando o novo tema) <code>Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}</code>5.aplicar o tema de novo e personalizado para o RP específicos (ou do RP)
<code>Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme</code>  
  
## <a name="home-realm-discovery"></a>Descoberta de território primário  
Para casa território gerada personalização veja [Personalizando as páginas do AD FS Sign-in](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Página de senha atualizada  
Para obter informações sobre como personalizar a página de senha de atualização, consulte [Personalizando as AD FS Sign-in páginas](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>IDs alternativas e personalização  
Os usuários podem fazer logon serviços de Federação do Active Directory (AD FS)-habilitado aplicativos usando qualquer forma de identificador do usuário que é aceito pelos serviços de domínio do Active Directory (AD DS). Isso inclui os nomes de entidade de segurança do usuário (UPNs) (johndoe@contoso.com) ou domínio qualificado nomes de conta sam (contoso\johndoe ou contoso.com\johndoe #).  Para obter mais informações sobre isso, consulte [ID Configurando logon alternativo.](Configuring-Alternate-Login-ID.md)  
  
Além disso convém personalizar a página de entrada do AD FS para dar aos usuários finais alguns dica sobre a ID de logon alternativo. Você pode fazer isso adicionando a descrição de página de entrada personalizada para mais informações, consulte [Personalizando as AD FS Sign-in páginas.](https://technet.microsoft.com/library/dn280950.aspx)   
  
Você também pode fazer isso Personalizando a cadeia de caracteres "faça logon com contas da organização" acima do campo de nome de usuário.  Para obter informações sobre isso, consulte [personalização avançada do AD FS Sign-in páginas](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
