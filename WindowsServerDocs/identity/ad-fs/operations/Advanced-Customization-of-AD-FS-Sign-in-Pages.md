---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: Personalização avançada de páginas de entrada AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 35d2c6dfaf509434aa8f61099b8440138904d450
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407720"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalização avançada de páginas de entrada AD FS

  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalização avançada de AD FS páginas\-de entrada  
AD FS no Windows Server 2012 R2 fornece suporte\-interno para personalizar a experiência de entrada\-. Para a maioria desses cenários, os cmdlets\-internos do Windows PowerShell são tudo o que é necessário.  É recomendável que você use os comandos\-internos do Windows PowerShell para personalizar elementos padrão para AD FS experiência\-de entrada sempre que possível.  Consulte [personalização de entrada do usuário do AD FS](AD-FS-user-sign-in-customization.md) para obter mais informações.  
  
Em alguns casos, AD FS administradores talvez queiram fornecer experiências de\-entrada adicionais que não são possíveis por meio dos comandos do PowerShell existentes que\-são fornecidos no box com o AD FS. Em determinadas instâncias, é \(viável nas diretrizes fornecidas abaixo\) para que os administradores personalizem ainda mais\-a experiência de entrada, adicionando lógica adicional ao **OnLoad. js** fornecido pelo AD FS e será executado em todas as páginas de AD FS.  
  
## <a name="things-to-know-before-you-start"></a>O que você precisa saber antes de começar  
  
-   Não há suporte para qualquer alteração que afete os parâmetros de protocolo de redirecionamento ou modificação com os quais AD FS funciona.
  
-   O OnLoad. js original, que vem com o tema da Web padrão, contém um código que manipula a renderização da página para diferentes fatores forma. É recomendável não modificar o conteúdo original OnLoad. js, mas acrescentar apenas o seu código ao OnLoad. js existente que manipula a lógica personalizada.  
  
-   O AD FS é fornecido com\-um tema da Web interno, que é chamado de padrão. Não é possível modificar o OnLoad. js do tema da Web padrão. Para atualizar o OnLoad. js, você precisa criar e usar um tema da Web personalizado para AD FS\-páginas de entrada.  Consulte [personalização de entrada do usuário do AD FS](AD-FS-user-sign-in-customization.md) para obter informações sobre como criar um tema da Web personalizado.  
  
-   O mesmo OnLoad. js será executado em todas as páginas \(do ADFS, por exemplo. página\-de logon baseada em formulário, página de descoberta de realm inicial e etc..\) Você precisa certificar-se de que o código em seu script seja executado apenas quando ele for projetado e não seja executado inesperadamente.  
  
-   Ao fazer referência a qualquer elemento HTML, certifique-se de sempre verificar a existência do elemento antes de agir no elemento. Isso fornece robustez e garante que a lógica personalizada não seja executada em páginas que não contenham esse elemento. Você pode simplesmente exibir o código-fonte HTML no AD FS\-páginas de entrada para exibir os elementos existentes.  
  
-   É altamente recomendável validar suas personalizações em um ambiente alternativo e testá-las antes de distribuí-las para servidores de produção AD FS. Isso reduz as chances de que os usuários finais sejam expostos a essas personalizações antes da validação.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personalizando a experiência de\-entrada de AD FS usando o OnLoad. js  
Use as etapas a seguir ao personalizar o OnLoad. js para o serviço de AD FS.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personalizando o OnLoad. js para o serviço de AD FS  
  
1.  Para adicionar sua lógica personalizada ao OnLoad. js, primeiro você precisa criar um tema da Web personalizado. O tema que é enviado\-para a\-\-caixa é chamado de padrão. Você pode exportar o tema padrão e usá-lo de forma a poder iniciar rapidamente. O cmdlet a seguir cria um tema da Web personalizado, que duplica o tema da Web padrão:  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  Em seguida, você pode exportar o tema da Web personalizado ou padrão para obter o arquivo OnLoad. js. Para exportar um tema da Web, use o seguinte cmdlet:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    Você encontrará OnLoad. js na pasta de script no diretório que você especificar no cmdlet Export acima e adicionará sua lógica personalizada ao script \(, consulte casos de uso na seção de exemplo abaixo.\)  
  
3.  Faça a modificação necessária para personalizar o OnLoad. js com base em sua necessidade.  
  
4.  Atualize o tema com o OnLoad. js modificado. Use o seguinte cmdlet para aplicar a atualização OnLoad. js ao tema da Web personalizado:  

     Para AD FS no Windows Server 2012 R2:  

    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';path="c:\theme\script\onload.js"}  
  
    ```  
    Para AD FS no Windows Server 2016:

     ```  
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\ADFStheme\script\onload.js"   
  
    ```  
  
5.  Para aplicar o tema da Web personalizado a AD FS, use o seguinte cmdlet:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Exemplos adicionais de personalização  
Veja a seguir os exemplos de código personalizado adicionado ao OnLoad. js para diferentes fins\-de ajuste fino. Ao adicionar o código personalizado, inclua sempre o código personalizado na parte inferior do OnLoad. js.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Exemplo 1: alterar a cadeia de caracteres "entrar com a conta institucional"  
A página de entrada\-\-baseada no formulário AD FS padrão tem um título de "entrar com sua conta institucional" acima das caixas de entrada do usuário.  
  
Se você quiser substituir essa cadeia de caracteres pela sua própria cadeia de caracteres, você pode adicionar o seguinte código a OnLoad. js.  
  
```  
// Sample code to change “Sign in with organizational account” string.  
  
// Check whether the loginMessage element is present on this page.  
var loginMessage = document.getElementById('loginMessage');  
if (loginMessage)  
{  
       // loginMessage element is present, modify its properties.  
       loginMessage.innerHTML = 'Your company description text';  
}  
  
```  
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Exemplo 2: aceitar o\-nome da conta Sam como um formato de logon em\-uma página\-de entrada baseada em AD FS formulário  
A página de entrada\-\-baseada no formulário padrão AD FS dá suporte ao formato de logon \(de\) nomes de entidade <strong>johndoe@contoso.com</strong> de usuário UPNs \(, por exemplo, \) ou Sam\-qualificadoparadomínio nomes\(de conta **Contoso\\davibarros** ou **contoso.com\\davibarros.** \) Caso todos os usuários venham do mesmo domínio e saibam apenas sobre nomes de conta Sam\-, talvez você queira dar suporte ao cenário em que os usuários podem entrar usando apenas nomes de conta Sam.\- Você pode adicionar o seguinte código a OnLoad. js para dar suporte a esse cenário, basta substituir o domínio "contoso.com" no exemplo abaixo pelo domínio que você deseja usar.  
  
```  
if (typeof Login != 'undefined'){  
    Login.submitLoginRequest = function () {   
    var u = new InputUtil();  
    var e = new LoginErrors();  
    var userName = document.getElementById(Login.userNameInput);  
    var password = document.getElementById(Login.passwordInput);  
    if (userName.value && !userName.value.match('[@\\\\]'))   
    {  
        var userNameValue = 'contoso.com\\' + userName.value;  
        document.forms['loginForm'].UserName.value = userNameValue;  
    }  
  
    if (!userName.value) {  
       u.setError(userName, e.userNameFormatError);  
       return false;  
    }  
  
    if (!password.value)   
    {  
        u.setError(password, e.passwordEmpty);  
        return false;  
    }  
    document.forms['loginForm'].submit();  
    return false;  
};  
}  
  
```  
  
## <a name="additional-references"></a>Referências adicionais 
[AD FS a personalização de entrada do usuário](AD-FS-user-sign-in-customization.md)  
  

