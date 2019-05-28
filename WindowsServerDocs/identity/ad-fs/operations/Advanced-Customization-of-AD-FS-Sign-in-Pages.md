---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: A personalização avançada de AD FS Sign-in Pages
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 01/16/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 73ff3fc6df872edd29735ee96c0918144250d5f1
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66190043"
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>A personalização avançada de AD FS Sign-in Pages

  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>A personalização avançada de entrada do AD FS\-em páginas  
O AD FS no Windows Server 2012 R2 fornece criados\-no suporte para personalizar o sinal\-na experiência. Para a maioria dos cenários, a interna\-no Windows PowerShell cmdlets são tudo o que é necessário.  É recomendável que você use o interno\-nos comandos do Windows PowerShell personalizem elementos padrão para o AD FS entrar\-na experiência sempre que possível.  Ver [personalização de entrada do usuário de AD FS](AD-FS-user-sign-in-customization.md) para obter mais informações.  
  
Em alguns casos, os administradores do AD FS talvez queira fornecer entrada adicional\-nas experiências que não são possíveis por meio de comandos do PowerShell que são fornecidos no\-caixa com o AD FS. Em determinadas circunstâncias, é viável \(dentro das diretrizes fornecidas abaixo\) para os administradores personalizem o sinal\-experiência ainda mais adicionando lógica adicional para **onload.js** que é fornecido pelo AD FS e será executado em todas as páginas do AD FS.  
  
## <a name="things-to-know-before-you-start"></a>É importante saber antes de começar  
  
-   Não há suporte para qualquer alteração que afeta os fluxos de redirecionamento ou modifica os parâmetros de protocolo do AD FS funciona com.
  
-   O onload.js original, aquela que acompanha o tema da web padrão, contém o código que manipula a renderização da página para diferentes fatores forma. É recomendável não modificar o conteúdo de onload.js original mas acrescentar apenas seu código para o onload.js existente que manipula a lógica personalizada.  
  
-   O AD FS é fornecido com um built\-no tema da web que é chamado de padrão. Você não pode modificar o onload.js do tema da web padrão. Para atualizar onload.js, você precisa criar e usar um tema da web personalizado para entrada do AD FS\-nas páginas.  Ver [personalização de entrada do usuário de AD FS](AD-FS-user-sign-in-customization.md) para obter informações sobre como criar um tema da web personalizado.  
  
-   O mesmo onload.js será executado em todas as páginas ADFS \(ex. formulário\-baseados em página de logon, a página home realm discovery e etc.\). Você precisa certificar-se de que o código em seu script é executado somente conforme ele é projetado e não seja executado inesperadamente.  
  
-   Ao fazer referência a qualquer elemento HTML, certifique-se de que você sempre verifique a existência do elemento antes de atuar no elemento. Isso fornece a robustez e garante que a lógica personalizada não será executada nas páginas que não contêm esse elemento. Você pode simplesmente exibir o código-fonte HTML na entrada do AD FS\-em páginas para exibir os elementos existentes.  
  
-   É altamente recomendável validar suas personalizações em um ambiente alternativo e testá-las antes de implantá-lo em produção servidores do AD FS. Isso reduz as chances dos usuários finais que está sendo expostos a essas personalizações antes da validação.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personalizando a entrada do AD FS\-na experiência usando onload.js  
Use as etapas a seguir ao personalizar o onload.js para o serviço do AD FS.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personalizando onload.js para o serviço do AD FS  
  
1.  Para adicionar sua lógica personalizada para onload.js, você precisa primeiro criar um tema da web personalizado. O tema que for enviado\-dos\-o\-caixa é chamada padrão. Você pode exportar o tema padrão e usá-lo de forma a poder iniciar rapidamente. O cmdlet a seguir cria um tema da web personalizado, que duplica o tema da web padrão:  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  Você pode exportar personalizado ou padrão tema da web para obter o arquivo onload.js. Para exportar um tema da web, use o seguinte cmdlet:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    Você encontrará onload.js sob a pasta de script no diretório que você especifica no cmdlet export acima e adiciona sua lógica personalizada para o script \(ver casos de uso na seção exemplo abaixo\).  
  
3.  Fazer a modificação necessária para personalizar onload.js com base na sua necessidade.  
  
4.  Atualize o tema com o onload.js modificado. Use o cmdlet a seguir para aplicar a atualização onload.js ao tema da web personalizado:  

     Para o AD FS no Windows Server 2012 R2:  

    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
    Para o AD FS no Windows Server 2016:

     ```  
    Set-AdfsWebTheme -TargetName custom -OnLoadScriptPath "c:\ADFStheme\script\onload.js"   
  
    ```  
  
5.  Para aplicar o tema da web personalizado para o AD FS, use o seguinte cmdlet:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Exemplos de personalização adicional  
A seguir estão exemplos de código personalizado adicionado ao onload.js para diferente fine\-fins de ajuste. Ao adicionar o código personalizado, por favor, sempre acrescente seu código personalizado na parte inferior do onload.js.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Exemplo 1: alterar a cadeia de caracteres "Entrar com conta organizacional"  
O padrão de formulário do AD FS\-logon baseado em\-na página tem um título de "Entrar com sua conta organizacional" acima caixas de entrada do usuário.  
  
Se você quiser substituir essa cadeia de caracteres com sua própria cadeia de caracteres, você pode adicionar o código a seguir para onload.js.  
  
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
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Exemplo 2: aceitar SAM\-nome da conta como um formato de logon em um formulário do AD FS\-logon baseado em\-na página  
O padrão de formulário do AD FS\-logon baseado em\-na página dá suporte ao formato de logon de nomes da entidade de usuário \(UPNs\) \(, por exemplo, **johndoe@contoso.com** \) ou domínio qualificado sam\-nomes de conta \( **contoso\\joãosilva** ou **contoso.com\\joãosilva**\). No caso de todos os usuários vêm do mesmo domínio e elas só conhecem sam\-nomes de conta, você talvez queira suportar o cenário em que os usuários podem entrar em usá-los sam\-somente nomes de conta. Você pode adicionar o código a seguir para onload.js para dar suporte a esse cenário, basta substituir o domínio "contoso.com" no exemplo abaixo com o domínio que você deseja usar.  
  
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
[AD FS Sign-personalização de usuário](AD-FS-user-sign-in-customization.md)  
  

