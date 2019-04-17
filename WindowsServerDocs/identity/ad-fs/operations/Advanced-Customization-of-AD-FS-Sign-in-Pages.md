---
ms.assetid: 882abec8-0189-4f73-99c5-792987168080
title: "Personalização do AD FS entrar páginas avançada"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/13/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: ea01d0ff2a38c4fef2f68091608d777d8412e91b
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalização do AD FS entrar páginas avançada

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2
  
## <a name="advanced-customization-of-ad-fs-sign-in-pages"></a>Personalização avançada do AD FS Sign\ em páginas  
AD FS no Windows Server 2012 R2 fornece suporte built\-in para personalizar a experiência de sign\. Para a maioria desses cenários, os cmdlets do Windows PowerShell built\-in são tudo o que é necessário.  É recomendável que você use os comandos do Windows PowerShell built\-in para personalizar elementos padrão para o AD FS sign\ experiência sempre que possível.  Consulte [personalização de entrada do usuário de AD FS](AD-FS-user-sign-in-customization.md) para obter mais informações.  
  
Em alguns casos, os administradores do AD FS talvez queira fornecer sign\ em experiências adicionais que não são possíveis com os comandos do PowerShell existentes fornecidos in\-caixa com o AD FS. Em determinadas circunstâncias, é possível \ (em below\ orientações fornecidas) para os administradores para personalizar a experiência sign\-in ainda mais adicionando lógica adicional para **onload.js** que é fornecido pelo AD FS e será executado em todas as páginas do AD FS.  
  
## <a name="things-to-know-before-you-start"></a>É importante saber antes de começar  
  
-   Qualquer alteração que afeta os fluxos de redirecionamento ou modifica os parâmetros de protocolo que o AD FS funciona com não é suportada.
  
-   O onload.js original, aquele que vem com o tema da web padrão, contém o código que manipula a renderização da página para diferentes fatores forma. É recomendável não para modificar o conteúdo de onload.js original, mas apenas acrescentar seu código para o onload.js existentes que manipula a lógica personalizada.  
  
-   AD FS vem com um tema built\ na web que é chamado de padrão. Você não pode modificar o onload.js do tema padrão da web. Para atualizar onload.js, você precisa criar e usar um tema personalizado da web para o AD FS sign\ em páginas.  Consulte [personalização de entrada do usuário de AD FS](AD-FS-user-sign-in-customization.md) para obter informações sobre como criar um tema personalizado da web.  
  
-   O mesmo onload.js será executado em todos os ADFS páginas \(ex. página de logon baseado em form\, página de descoberta de território primário e etc. \). Você precisa verificar se que o código em seu script é executado somente conforme ele é projetado e não é executado inesperadamente.  
  
-   Ao fazer referência a qualquer elemento HTML, certifique-se de que você sempre verificar a existência do elemento antes atuando no elemento. Isso fornece robustez e garante que a lógica personalizada não será executada em páginas que não contêm esse elemento. Você pode simplesmente exibir o código-fonte HTML sobre as páginas de sign\ do AD FS para exibir os elementos existentes.  
  
-   É altamente recomendável para validar suas personalizações em um ambiente alternativo e testá-los antes de implantá-lo em produção servidores do AD FS. Isso reduz a probabilidade dos usuários finais sejam expostos a essas personalizações antes da validação.  
  
## <a name="customizing-the-ad-fs-sign-in-experience-by-using-onloadjs"></a>Personalizando a experiência de sign\ AD FS usando onload.js  
Use as etapas a seguir ao personalizar o onload.js para o serviço do AD FS.  
  
#### <a name="customizing-onloadjs-for-the-ad-fs-service"></a>Personalizando onload.js para o serviço do AD FS  
  
1.  Para adicionar a lógica personalizada para onload.js, você precisa primeiro criar um tema personalizado da web. O tema é enviado out\-of\-the\-box é chamado de padrão. Você pode exportar o tema padrão e usá-lo para que você possa começar rapidamente. O seguinte cmdlet cria um tema personalizado na web, que duplica o tema da web padrão:  
  
    ```  
    New-AdfsWebTheme –Name custom –SourceName default  
  
    ```  
  
2.  Você pode exportar personalizado ou tema da web para obter onload.js arquivo padrão. Para exportar um tema da web, use o seguinte cmdlet:  
  
    ```  
    Export-AdfsWebTheme –Name default –DirectoryPath c:\theme  
  
    ```  
  
    Você encontrará onload.js sob a pasta de script na pasta que você especifica no cmdlet export acima e adiciona a lógica personalizada para o script \ (veja casos de uso do exemplo seção below\).  
  
3.  Verifique a modificação necessária para personalizar onload.js com base em suas necessidades.  
  
4.  Atualize o tema com o onload.js modificado. Use o seguinte cmdlet para aplicar onload.js a atualização para o tema da web personalizado:  
  
    ```  
    Set-AdfsWebTheme -TargetName custom -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}  
  
    ```  
  
5.  Para aplicar o tema da web personalizado para o AD FS, use o seguinte cmdlet:  
  
    ```  
    Set-AdfsWebConfig -ActiveThemeName custom  
    ```  
  
## <a name="additional-customization-examples"></a>Exemplos de personalização adicional  
A seguir estão exemplos de código personalizado adicionado ao onload.js para fins de ajuste fine\ diferentes. Ao adicionar o código personalizado, por favor, sempre acrescente código personalizado na parte inferior da onload.js.  
  
### <a name="example-1-change-sign-in-with-organizational-account-string"></a>Exemplo 1: alterar a cadeia de caracteres "Entrar com a conta organizacional"  
A AD FS com base em form\ sign\ na página padrão tem um título de "Entrar com sua conta organizacional" acima caixas de entrada do usuário.  
  
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
  
### <a name="example-2-accept-sam-account-name-as-a-login-format-on-an-ad-fs-form-based-sign-in-page"></a>Exemplo 2: aceitar o nome da conta SAM\ como um formato de logon um AD FS com base em form\ sign\ na página  
O padrão do AD FS com base em form\ sign\ página dá suporte à logon formato de nomes de usuário Principal \(UPNs\) \(for example, ** johndoe@contoso.com **\) ou domínio qualificado nomes de contas sam\ \ (**contoso\\johndoe** ou **contoso.com\\johndoe**\). No caso de todos os seus usuários é proveniente do mesmo domínio e que eles conhecem apenas nomes de contas sam\, convém suporte ao cenário em que os usuários podem entrar em usá-los apenas nomes de conta sam\. Você pode adicionar o código a seguir para onload.js para permitir esse cenário, basta substituir o domínio "contoso.com" no exemplo a seguir com o domínio que você deseja usar.  
  
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
[AD FS usuário entrar personalização](AD-FS-user-sign-in-customization.md)  
  

