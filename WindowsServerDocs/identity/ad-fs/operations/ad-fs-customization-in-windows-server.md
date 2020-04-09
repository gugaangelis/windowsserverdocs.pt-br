---
ms.assetid: 25f5aff1-6abf-4dea-b531-f1d9943bc181
title: AD FS personalização no Windows Server 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 3f46ea97034c382d1846ff95bb0974307943bbc5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860089"
---
# <a name="ad-fs-customization-in-windows-server-2016"></a>AD FS personalização no Windows Server 2016


Em resposta a comentários de organizações que usam AD FS, adicionamos ferramentas adicionais para personalizar a experiência de entrada do usuário para aplicativos individuais protegidos pelo AD FS.  
Além de especificar conteúdo da Web por aplicativo, como texto de descrição e links, agora você pode especificar temas inteiros da Web por aplicativo.  Isso inclui o logotipo, a ilustração, as folhas de estilo ou um arquivo OnLoad. js inteiro.  
  
## <a name="global-settings"></a>Configurações globais    
Para configurações globais gerais, você pode consultar [a personalização das páginas de entrada AD FS](https://technet.microsoft.com/library/dn280950.aspx) fornecidas com o AD FS no Windows Server 2012 R2.  
  
## <a name="pre-requisites"></a>Pré-requisitos  
Os pré-requisitos a seguir são necessários antes de tentar os procedimentos descritos neste documento.  
  
-   AD FS no Windows Server 2016 TP4 ou posterior  
  
## <a name="configure-ad-fs-relying-parties"></a>Configurar AD FS terceiras partes confiáveis  
Por elementos da Web de entrada de terceira parte confiável, os temas podem ser configurados usando os exemplos do PowerShell abaixo:  
  
### <a name="customize-messages"></a>Personalizar mensagens  
  
```  
PS C:\>Set-AdfsRelyingPartyWebContent  
    -TargetRelyingPartyName "<RP trust Name>"  
    -CompanyName "This text appears in place of the federation service display name"  
    -OrganizationalNameDescriptionText "This text appears right below the company name"  
    -SignInPageDescription "This text appears below the credential prompt"  
```  
  
### <a name="customize-company-name-logo-and-image"></a>Personalizar o nome da empresa, o logotipo e a imagem  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -Logo @{path="C:\Images\applogo.png"}  
    -Illustration @{path="C:\Images\appillustration.jpg"}  
```  
  
### <a name="customize-entire-page"></a>Personalizar página inteira  
  
```  
PS C:\>Set-AdfsRelyingPartyWebTheme  
    -TargetRelyingPartyName "<RP trust Name>"  
    -OnLoadScriptPath @{path="c:\scripts\adfstheme\onload.js"}  
```  
  
## <a name="custom-themes-and-advanced-custom-themes"></a>Temas personalizados e temas personalizados avançados  
  
Para temas personalizados, consulte [Personalizando as páginas de entrada AD FS](https://technet.microsoft.com/library/dn280950.aspx) e a [personalização avançada de AD FS páginas de entrada.](https://technet.microsoft.com/library/dn636121.aspx)  
  
## <a name="assigning-custom-web-themes-per-rp"></a>Atribuindo temas da Web personalizados por RP  
  
Para atribuir um tema personalizado por RP, use o seguinte procedimento:  
  
1. Crie um novo tema como uma cópia para o tema padrão, global no AD FS  
`New-AdfsWebTheme -Name AppSpecificTheme -SourceName default`  
2. Exportar o tema para personalização  
`Export-AdfsWebTheme -Name AppSpecificTheme -DirectoryPath c:\appspecifictheme`  
3. Personalizar arquivos de tema (imagens, CSS, OnLoad. js) – em seu editor favorito ou substituir o arquivo  
4. Importar arquivos personalizados do sistema de arquivos para AD FS (direcionamento para o novo tema)  
`Set-AdfsWebTheme -TargetName AppSpecificTheme -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';Path="c:\appspecifictheme\script\onload.js"}`  
5. Aplicar o novo tema personalizado ao RP específico (ou ao RP)  
`Set-AdfsRelyingPartyWebTheme -TargetRelyingPartyName urn:app1 -SourceWebThemeName AppSpecificTheme`  
  
## <a name="home-realm-discovery"></a>Descoberta de realm inicial  
Para obter a personalização da descoberta de realm inicial [, consulte Personalizando as páginas de entrada do AD FS](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="updated-password-page"></a>Página senha atualizada  
Para obter informações sobre como personalizar a página de atualização de senha, consulte [Personalizando o AD FS páginas de entrada](https://technet.microsoft.com/library/dn280950.aspx).  
  
## <a name="customizing-and-alternate-ids"></a>Personalizando e identificando IDs alternativas  
Os usuários podem entrar em aplicativos habilitados para Serviços de Federação do Active Directory (AD FS) (AD FS) usando qualquer forma de identificador de usuário aceita pelo Active Directory Domain Services (AD DS). Isso inclui nomes de entidade de usuário (UPNs) (johndoe@contoso.com) ou nomes de conta Sam qualificados de domínio (contoso\johndoe ou contoso. com\johndoe).  Para obter mais informações sobre isso, consulte [Configurando a ID de logon alternativa.](Configuring-Alternate-Login-ID.md)  
  
Além disso, você pode querer personalizar a página de entrada AD FS para dar aos usuários finais alguma dica sobre a ID de logon alternativa. Você pode fazer isso adicionando a descrição da página de entrada personalizada para obter mais informações consulte [Personalizando o AD FS páginas de entrada.](https://technet.microsoft.com/library/dn280950.aspx)   
  
Você também pode fazer isso Personalizando a cadeia de caracteres "entrar com a conta institucional" acima do campo nome de usuário.  Para obter informações sobre isso, consulte [personalização avançada de AD FS páginas de entrada](https://technet.microsoft.com/library/dn636121.aspx).  

## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
