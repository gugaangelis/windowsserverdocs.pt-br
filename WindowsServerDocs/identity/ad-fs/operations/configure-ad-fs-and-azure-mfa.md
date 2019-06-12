---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configurar o AD FS 2016 e o Azure MFA
description: ''
ms.author: billmath
author: billmath
manager: mtillman
ms.date: 01/28/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 62b366b8fa388319a758ab853d28d1c49cb1bf06
ms.sourcegitcommit: a3958dba4c2318eaf2e89c7532e36c78b1a76644
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2019
ms.locfileid: "66719721"
---
# <a name="configure-azure-mfa-as-authentication-provider-with-ad-fs"></a>Configurar o MFA do Azure como provedor de autenticação com o AD FS

Se sua organização for federada com o Azure AD, você pode usar a autenticação multifator do Azure para proteger recursos do AD FS, tanto localmente quanto na nuvem. O MFA do Azure permite que você eliminar senhas e fornecem uma maneira mais segura de autenticar.  Começando com o Windows Server 2016, agora você pode configurar o Azure MFA para autenticação primária ou usá-lo como um provedor de autenticação adicional. 
  
Ao contrário de com o AD FS no Windows Server 2012 R2, o adaptador do AD FS 2016 MFA do Azure integra-se diretamente com o Azure AD e não requer um servidor Azure MFA local.   O adaptador do MFA do Azure é integrado no Windows Server 2016, e não é necessário para instalação adicional.


## <a name="registering-users-for-azure-mfa-with-ad-fs"></a>Registrar usuários para MFA do Azure com o AD FS

O AD FS não oferece suporte embutido &#34;prova para cima&#34;, ou o registro de informações de verificação de segurança do Azure MFA, como o número de telefone ou aplicativo móvel. Isso significa que os usuários devem obter pré-listagem backup visitando [ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx) antes de usar o Azure MFA para autenticar em aplicativos do AD FS. Quando um usuário que tem não ainda à prova de backup no Azure AD tenta autenticar com o Azure MFA no AD FS, elas receberão um erro de AD FS.  Como administrador do AD FS, você pode personalizar essa experiência de erro para orientar o usuário para a página proofup em vez disso.  Você pode fazer isso usando a personalização de onload.js para detectar a cadeia de caracteres de mensagem de erro dentro da página do AD FS e mostrar uma nova mensagem para orientar os usuários a visitar [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup), em seguida, tente novamente a autenticação. Para obter orientações detalhadas, consulte "Personalizar o AD FS página da web para orientar os usuários a registrar os métodos de verificação de MFA" abaixo neste artigo.

>[!NOTE]
> Anteriormente, os usuários precisavam para autenticar com o MFA para registro (visitando [ https://account.activedirectory.windowsazure.com/Proofup.aspx ](https://account.activedirectory.windowsazure.com/Proofup.aspx), por exemplo, por meio do atalho [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup)).  Agora, um usuário do AD FS que ainda não foram registrados informações de verificação de MFA pode acessar o Azure AD&#34;página de prova de s por meio do atalho [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) usando somente a autenticação primária (como a autenticação integrada do Windows ou nome de usuário e senha do AD FS por meio de páginas da web).  Se o usuário não tem nenhum método de verificação configurado, o Azure AD executará o registro embutido no qual o usuário vê a mensagem &#34;seu administrador exigiu que você configure esta conta para verificação de segurança adicional&#34;, e o usuário pode, em seguida, Selecione esta opção para &#34;configurar agora&#34;.
> Os usuários que já têm pelo menos um método de verificação de MFA configurado ainda serão solicitados a fornecer MFA ao visitar a página proofup.

## <a name="recommended-deployment-topologies"></a>Topologias de implantação recomendada

### <a name="azure-mfa-as-primary-authentication"></a>MFA do Azure como autenticação primária

Há alguns motivos para usar o Azure MFA como autenticação primária com o AD FS:

 - Para evitar senhas para entrar no AD do Azure, Office 365 e outros aplicativos do AD FS
 - Para proteger a senha com base na entrada, exigindo um fator adicional, como o código de verificação antes da senha

Se você quiser usar o Azure MFA como um método de autenticação primária no AD FS para atingir esses benefícios, você provavelmente também precisará manter a capacidade de usar incluindo o acesso condicional do Azure AD &#34;true MFA&#34; , solicitando fatores adicionais no AD FS.

Agora você pode fazer isso definindo a configuração de domínio do AD do Azure faça a MFA no local (configuração de &#34;SupportsMfa&#34; para $True).  Nessa configuração, do AD FS pode ser solicitado pelo AD do Azure para realizar a autenticação adicional ou &#34;true MFA&#34; para cenários de acesso condicional que precisam dele.  

Conforme descrito acima, qualquer usuário do AD FS que ainda não foram registrados (informações de verificação de MFA configuradas) deve ser solicitado por meio de uma página de erro personalizada do AD FS para visitar [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) para configurar informações de verificação, em seguida, tentativa de logon do AD FS novamente.  
Como a MFA do Azure como primário é considerado um único fator, após a configuração inicial, os usuários serão necessário fornecer um fator adicional para gerenciar ou atualizar suas informações de verificação do AD do Azure ou para acessar outros recursos que exigem MFA.

>[!NOTE]
> Com o ADFS 2019, você deve fazer uma modificação para o tipo de declaração de ancoragem para a relação de confiança do provedor de declarações do Active Directory e modificar isso a partir de windowsaccountname para UPN. Execute o cmdlet do PowerShell fornecido abaixo. Isso não tem impacto sobre o funcionamento interno do farm do AD FS. Você pode perceber que alguns usuários podem ser solicitados novamente as credenciais, depois que essa alteração é feita. Depois de entrar novamente, os usuários finais não verão nenhuma diferença. 

```powershell
Set-AdfsClaimsProviderTrust -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -TargetName "Active Directory"
```

### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure MFA como autenticação adicional para o Office 365

Anteriormente, se você quisesse ter Azure MFA como era de um método de autenticação adicional no AD FS para o Office 365 ou outras terceiras partes confiáveis, a melhor opção configurar o Azure AD para MFA, na qual a autenticação primária é executada localmente no AD FS e MFA é tr iggered pelo AD do Azure. Agora, você pode usar o Azure MFA como autenticação adicional no AD FS quando a configuração de SupportsMfa do domínio é definida como $True.  

Conforme descrito acima, qualquer usuário do AD FS que ainda não foram registrados (informações de verificação de MFA configuradas) deve ser solicitado por meio de uma página de erro personalizada do AD FS para visitar [ https://aka.ms/mfasetup ](https://aka.ms/mfasetup) para configurar informações de verificação, em seguida, tentativa de logon do AD FS novamente.  

## <a name="pre-requisites"></a>Pré-requisitos

Os seguintes pré-requisitos são necessários ao usar o Azure MFA para autenticação com o AD FS:  
  
- Uma [assinatura do Azure com o Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/).  
- [Autenticação multifator do Azure](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) 


> [!NOTE]
> Azure AD e o Azure MFA estão incluídas no Azure AD Premium e o Enterprise Mobility Suite (EMS).  Se você tiver uma dessas assinaturas individuais não é necessário.

- Um ambiente do AD FS do Windows Server 2016 no local.  
   - O servidor precisa ser capaz de se comunicar com as seguintes URLs pela porta 443.
      - https://adnotifications.windowsazure.com
      - https://login.microsoftonline.com
- O seu ambiente local é [federado com o Azure AD.](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Módulo do Azure Active Directory do Windows para o Windows PowerShell](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).  
- Permissões de administrador global em sua instância do AD do Azure para configurá-lo usando o PowerShell do Azure AD.  
- Credenciais de administrador corporativo para configurar o farm do AD FS para o Azure MFA.  
  
## <a name="configure-the-ad-fs-servers"></a>Configurar os servidores do AD FS

Para concluir a configuração para o Azure MFA para AD FS, você precisa configurar cada servidor do AD FS usando as etapas descritas. 

>[!NOTE]
>Certifique-se de que essas etapas são executadas em **todos os** servidores do AD FS no farm. Se você tiver vários servidores do AD FS no farm, você pode executar a configuração necessária remotamente usando o PowerShell do Azure AD.  

### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Etapa 1: Gerar um certificado para o Azure MFA em cada servidor do AD FS usando o `New-AdfsAzureMfaTenantCertificate` cmdlet

A primeira coisa que você precisa fazer é gerar um certificado para o Azure MFA usar.  Isso pode ser feito usando o PowerShell.  O certificado gerado pode ser encontrado no repositório de certificados de computadores locais, e ele é marcado com um nome de entidade que contém a TenantID para o diretório do Azure AD.

![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  

Observe que o TenantID é o nome do seu diretório do AD do Azure.  Use o seguinte cmdlet do PowerShell para gerar o novo certificado.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  

![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-the-azure-multi-factor-auth-client-service-principal"></a>Etapa 2: Adicionar as novas credenciais para a entidade de serviço do cliente do Azure multi-factor Auth

Para permitir que os servidores do AD FS para se comunicar com o cliente de autenticação multifator do Azure, você precisará adicionar as credenciais para a entidade de serviço para o cliente de autenticação multifator do Azure. Os certificados gerados usando o `New-AdfsAzureMFaTenantCertificate` cmdlet servirá como essas credenciais. Faça o seguinte usando o PowerShell para adicionar as novas credenciais para a entidade de serviço do cliente do Azure multi-factor Auth.  

> [!NOTE]
> Para concluir esta etapa, você precisa se conectar à sua instância do AD do Azure com o PowerShell usando `Connect-MsolService`.  Essas etapas pressupõem que você já se conectou por meio do PowerShell.  Para obter informações, consulte [ `Connect-MsolService`.](https://msdn.microsoft.com/library/dn194123.aspx)  

**Defina o certificado como a nova credencial contra o cliente de autenticação multifator do Azure**  

`New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64`

> [!IMPORTANT]
> Este comando precisa ser executado em todos os servidores do AD FS no farm.  MFA do Azure AD falhará em servidores que não possuírem o certificado definido como a nova credencial contra o cliente de autenticação multifator do Azure.

> [!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 é o GUID do cliente de autenticação multifator do Azure.  
  
## <a name="configure-the-ad-fs-farm"></a>Configurar o Farm do AD FS  
  
Depois de ter concluído a seção anterior em cada servidor do AD FS, você precisará executar o `Set-AdfsAzureMfaTenant` cmdlet.  
  
Esse cmdlet precisa ser executado apenas uma vez para um farm do AD FS.  Use o PowerShell para concluir esta etapa.
  
> [!NOTE]  
> Você precisará reiniciar o serviço do AD FS em cada servidor no farm, antes que essas alterações entrarão em vigor.  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  

![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
Depois disso, você verá que o Azure MFA está disponível como um método de autenticação primária para intranet e extranet uso.    
  
![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Renovar e gerenciar o AD FS Azure MFA certificados

As diretrizes a seguir apresenta como gerenciar os certificados Azure MFA em seus servidores do AD FS.
Por padrão, quando você configurar o AD FS com o Azure MFA, os certificados gerados por meio de `New-AdfsAzureMfaTenantCertificate` cmdlet do PowerShell são válidos por 2 anos.  Para determinar como fechar para expiração de seus certificados são e, em seguida, para renovar e instalar novos certificados, use o procedimento a seguir.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Avaliar a data de validade do certificado do AD FS do Azure MFA

Em cada servidor do AD FS, no computador local meu repositório, haverá um certificado autoassinado com &#34;UO = Microsoft AD FS do Azure MFA&#34; no assunto e emissor.  Esse é o certificado de MFA do Azure.  Verifique se o período de validade deste certificado em cada servidor do AD FS para determinar a data de validade.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Criar novo certificado do AD FS do Azure MFA em cada servidor do AD FS

Se o período de validade de seus certificados está se aproximando do fim, inicie o processo de renovação, gerando um novo certificado de MFA do Azure em cada servidor do AD FS. Em uma janela de comando do PowerShell, gere um novo certificado em cada servidor do AD FS usando o seguinte cmdlet:

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

Como resultado desse cmdlet, será gerado um novo certificado é válido a partir de 2 dias no futuro de 2 dias + 2 anos.  Operações do AD FS e o Azure MFA não serão afetadas por esse cmdlet ou o novo certificado. (Observação: o atraso de 2 dias é intencional e fornece o tempo para executar as etapas abaixo para configurar o novo certificado no locatário antes do início do AD FS usá-lo para o Azure MFA.)

### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configurar cada novo certificado do AD FS do Azure MFA no locatário do AD do Azure

Usando o módulo do PowerShell do Azure AD, para cada novo certificado (em cada servidor do AD FS), atualize suas configurações de locatário do AD do Azure da seguinte maneira (Observação: você deve primeiro se conectar ao locatário usando `Connect-MsolService` para executar os comandos a seguir).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $newcert
```

`$certbase64` é o novo certificado.  O certificado codificado em base64 pode ser obtido por meio da exportação de certificado (sem a chave privada) como um DER codificada em arquivo e abrindo no Notepad.exe, em seguida, copiando/colando à sessão do PowerShell e atribuir à variável `$certbase64`.

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Verifique se que o novo certificado será usado para o Azure MFA

Depois que os novos certificados tornam-se válida, o AD FS coletá-los e começar a usar cada respectivo certificado para o Azure MFA em algumas horas para um dia.  Quando isso ocorre, você verá um evento registrado no log de eventos do administrador do AD FS com as seguintes informações em cada servidor:

```
Log Name:      AD FS/Admin
Source:        AD FS
Date:          2/27/2018 7:33:31 PM
Event ID:      547
Task Category: None
Level:         Information
Keywords:      AD FS
User:          DOMAIN\adfssvc
Computer:      ADFS.domain.contoso.com
Description:
The tenant certificate for Azure MFA has been renewed.  

TenantId: contoso.onmicrosoft.com.
Old thumbprint: 7CC103D60967318A11D8C51C289EF85214D9FC63.
Old expiration date: 9/15/2019 9:43:17 PM.
New thumbprint: 8110D7415744C9D4D5A4A6309499F7B48B5F3CCF.
New expiration date: 2/27/2020 2:16:07 AM.
```

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personalizar a página da web do AD FS para orientar os usuários a registrar os métodos de verificação de MFA

Use os exemplos a seguir personalizar as páginas da web do AD FS para os usuários que ainda não tiveram aprovadas para cima (configurado informações de verificação de MFA).

### <a name="find-the-error"></a>Localize o erro

Em primeiro lugar, há algumas mensagens de erro diferente que do AD FS será retornado no caso em que o usuário não tem informações de verificação.
Se você estiver usando o Azure MFA como autenticação primária, o usuário não proofed será exibida uma página de erro de AD FS que contém as seguintes mensagens:
```
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information.
        </div>
```
Quando o Azure AD como autenticação adicional que está sendo tentado, o usuário proofed não verá uma página de erro do AD FS que contém as seguintes mensagens:
```
<div id='mfaGreetingDescription' class='groupMargin'>For security reasons, we require additional information to verify your account (mahesh@jenfield.net)</div>
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            The selected authentication method is not available for &#39;username@contoso.com&#39;. Choose another authentication method or contact your system administrator for details.
        </div>
```

### <a name="catch-the-error-and-update-the-page-text"></a>Capturar o erro e atualizar o texto da página

Para capturar o erro e mostrar ao usuário Diretrizes personalizadas simplesmente acrescentar o javascript ao final do arquivo onload.js que faz parte do tema da web do AD FS.  Isso permite que você faça o seguinte:
 - Pesquise as identificação cadeias de caracteres de erro
 - fornece conteúdo da web personalizado.  

> [!NOTE]
> Para obter orientação em geral sobre como personalizar o arquivo onload.js, consulte o artigo [Advanced Customization of AD FS Sign-in Pages](advanced-customization-of-ad-fs-sign-in-pages.md).

Aqui está um exemplo simples, você talvez queira estender:

1. Abra o Windows PowerShell em seu servidor primário do AD FS e criar um novo tema de Web do AD FS, executando o seguinte comando:
    
    ``` PowerShell
        New-AdfsWebTheme –Name ProofUp –SourceName default
    ``` 
2. Em seguida, crie a pasta e exportar o tema da Web do AD FS padrão:

    ``` PowerShell
       New-Item -Path 'c:\Theme' -ItemType Directory;Export-AdfsWebTheme –Name default –DirectoryPath c:\Theme
    ```
3. Abra o arquivo C:\Theme\script\onload.js em um editor de texto
4. Acrescente o seguinte código ao final do arquivo onload.js
    
    ``` JavaScript
    //Custom Code
    //Customize MFA exception
    //Begin

    var domain_hint = "<YOUR_DOMAIN_NAME_HERE>";
    var mfaSecondFactorErr = "The selected authentication method is not available for";
    var mfaProofupMessage = "You will be automatically redirected in 5 seconds to set up your account for additional security verification. Once you have completed the setup, please return to the application you are attempting to access.<br><br>If you are not redirected automatically, please click <a href='{0}'>here</a>."
    var authArea = document.getElementById("authArea");
    if (authArea) {
        var errorMessage = document.getElementById("errorMessage");
        if (errorMessage) {
            if (errorMessage.innerHTML.indexOf(mfaSecondFactorErr) >= 0) {

                //Hide the error message
                var openingMessage = document.getElementById("openingMessage");
                if (openingMessage) {
                    openingMessage.style.display = 'none'
                }
                var errorDetailsLink = document.getElementById("errorDetailsLink");
                if (errorDetailsLink) {
                    errorDetailsLink.style.display = 'none'
                }

                //Provide a message and redirect to Azure AD MFA Registration Url
                var mfaRegisterUrl = "https://account.activedirectory.windowsazure.com/proofup.aspx?proofup=1&whr=" + domain_hint;
                errorMessage.innerHTML = "<br>" + mfaProofupMessage.replace("{0}", mfaRegisterUrl);
                window.setTimeout(function () { window.location.href = mfaRegisterUrl; }, 5000);
            }
        }
    }

    //End Customize MFA Exception
    //End Custom Code
    ```
    > [!IMPORTANT]
    > Você precisa alterar "< YOUR_DOMAIN_NAME_HERE >"; Para usar seu nome de domínio. Por exemplo: `var domain_hint = "contoso.com";`
    
5. Salve o arquivo onload.js
6. Importe o arquivo de onload.js para o tema personalizado, digitando o seguinte comando do Windows PowerShell:
    
    ``` PowerShell
    Set-AdfsWebTheme -TargetName ProofUp -AdditionalFileResource @{Uri=’/adfs/portal/script/onload.js’;path="c:\theme\script\onload.js"}
    ```
7. Por fim, aplique o tema de Web personalizados do AD FS, digitando o seguinte comando do Windows PowerShell:
    
    ``` PowerShell
    Set-AdfsWebConfig -ActiveThemeName "ProofUp"
    ```

## <a name="next-steps"></a>Próximas etapas

[Gerenciar protocolos TLS/SSL e Cipher Suites usados pelo AD FS e o Azure MFA](manage-ssl-protocols-in-ad-fs.md)
