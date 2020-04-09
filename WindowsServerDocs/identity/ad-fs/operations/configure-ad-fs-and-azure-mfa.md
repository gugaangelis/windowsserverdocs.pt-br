---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configurar o AD FS 2016 e o Azure MFA
ms.author: billmath
author: billmath
manager: mtillman
ms.date: 01/28/2019
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: d519b47d048068ad53e4f11a6b64621ab5f232b1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855309"
---
# <a name="configure-azure-mfa-as-authentication-provider-with-ad-fs"></a>Configurar o Azure MFA como provedor de autenticação com AD FS

Se sua organização for federada com o Azure AD, você poderá usar a autenticação multifator do Azure para proteger AD FS recursos, tanto localmente quanto na nuvem. O Azure MFA permite que você elimine senhas e forneça uma maneira mais segura de autenticar.  A partir do Windows Server 2016, agora você pode configurar o Azure MFA para autenticação primária ou usá-lo como um provedor de autenticação adicional. 
  
Ao contrário do AD FS no Windows Server 2012 R2, o adaptador do Azure MFA AD FS 2016 é integrado diretamente ao Azure AD e não requer um servidor Azure MFA local.   O adaptador do Azure MFA é integrado ao Windows Server 2016 e não há necessidade de instalação adicional.


## <a name="registering-users-for-azure-mfa-with-ad-fs"></a>Registrando usuários para o Azure MFA com AD FS

AD FS não dá suporte à &#34;verificação embutida&#34;ou ao registro de informações de verificação de segurança do Azure MFA, como número de telefone ou aplicativo móvel. Isso significa que os usuários devem ser reprovados visitando [https://account.activedirectory.windowsazure.com/Proofup.aspx](https://account.activedirectory.windowsazure.com/Proofup.aspx) antes de usar o Azure MFA para autenticar para AD FS aplicativos. Quando um usuário que ainda não foi reprovado no Azure AD tenta se autenticar com o Azure MFA em AD FS, ele receberá um erro de AD FS.  Como administrador de AD FS, você pode personalizar essa experiência de erro para orientar o usuário na página do proofup em vez disso.  Você pode fazer isso usando a personalização de OnLoad. js para detectar a cadeia de caracteres de mensagem de erro na página AD FS e mostrar uma nova mensagem para orientar os usuários a visitarem [https://aka.ms/mfasetup](https://aka.ms/mfasetup)e tentar novamente a autenticação. Para obter diretrizes detalhadas, consulte "personalizar a página da Web AD FS para guiar os usuários a registrar os métodos de verificação de MFA" abaixo neste artigo.

>[!NOTE]
> Anteriormente, os usuários eram obrigados a se autenticar com o MFA para registro (visitando [https://account.activedirectory.windowsazure.com/Proofup.aspx](https://account.activedirectory.windowsazure.com/Proofup.aspx), por exemplo, por meio do atalho [https://aka.ms/mfasetup](https://aka.ms/mfasetup)).  Agora, um usuário AD FS que ainda não registrou informações de verificação de MFA pode acessar&#34;a página do Azure ad s proofup por meio do atalho [https://aka.ms/mfasetup](https://aka.ms/mfasetup) usando a autenticação primária (como a autenticação integrada do Windows ou o nome de usuário e a senha por meio das páginas da Web do AD FS).  Se o usuário não tiver nenhum método de verificação configurado, o Azure AD executará o registro embutido no qual &#34;o usuário vê a mensagem de que seu administrador exigiu que você configurou essa conta para verificação&#34;de segurança adicional &#34;e o usuário pode&#34;selecionar para configurá-la agora.
> Os usuários que já tiverem pelo menos um método de verificação de MFA configurado ainda serão solicitados a fornecer MFA ao visitar a página proofup.

## <a name="recommended-deployment-topologies"></a>Topologias de implantação recomendadas

### <a name="azure-mfa-as-primary-authentication"></a>Azure MFA como autenticação primária

Há alguns ótimos motivos para usar o Azure MFA como autenticação primária com o AD FS:

 - Para evitar senhas para entrar no Azure AD, Office 365 e outros aplicativos AD FS
 - Para proteger a entrada baseada em senha exigindo um fator adicional, como o código de verificação anterior à senha

Se você quiser usar o Azure MFA como um método de autenticação principal no AD FS para atingir esses benefícios, provavelmente também desejará manter a capacidade de usar o acesso condicional do &#34;Azure ad&#34; , incluindo a verdadeira MFA, solicitando fatores adicionais no AD FS.

Agora você pode fazer isso Configurando a configuração de domínio do Azure AD para fazer MFA local &#34;(&#34; Configurando SupportsMfa para $true).  Nessa configuração, AD FS pode ser solicitado pelo Azure AD para executar autenticação adicional ou &#34;verdadeira MFA&#34; para cenários de acesso condicional que o exigem.  

Conforme descrito acima, qualquer usuário AD FS que ainda não tenha se registrado (informações de verificação de MFA configuradas) deverá ser solicitado por meio de uma página de erro de AD FS personalizada para visitar [https://aka.ms/mfasetup](https://aka.ms/mfasetup) para configurar informações de verificação e, em seguida, tentar novamente AD FS logon.  
Como o Azure MFA como primário é considerado um único fator, após a configuração inicial, os usuários precisarão fornecer um fator adicional para gerenciar ou atualizar suas informações de verificação no Azure AD ou para acessar outros recursos que exigem MFA.

>[!NOTE]
> Com o ADFS 2019, é necessário fazer uma modificação no tipo de declaração de ancoragem para a confiança do provedor de declarações Active Directory e modificá-lo do windowsaccountname para o UPN. Execute o cmdlet do PowerShell fornecido abaixo. Isso não afeta o funcionamento interno do farm de AD FS. Você pode observar que alguns usuários podem ser solicitados a fornecer credenciais novamente quando essa alteração for feita. Depois de fazer logon novamente, os usuários finais não verão nenhuma diferença. 

```powershell
Set-AdfsClaimsProviderTrust -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -TargetName "Active Directory"
```

### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure MFA como autenticação adicional para o Office 365

Anteriormente, se você queria ter o Azure MFA como um método de autenticação adicional no AD FS para o Office 365 ou outras terceiras partes confiáveis, a melhor opção era configurar o Azure AD para fazer a MFA composta, na qual a autenticação principal é executada localmente no AD FS e a MFA é disparada pelo Azure AD. Agora, você pode usar o Azure MFA como autenticação adicional no AD FS quando a configuração de SupportsMfa de domínio estiver definida como $True.  

Conforme descrito acima, qualquer usuário AD FS que ainda não tenha se registrado (informações de verificação de MFA configuradas) deverá ser solicitado por meio de uma página de erro de AD FS personalizada para visitar [https://aka.ms/mfasetup](https://aka.ms/mfasetup) para configurar informações de verificação e, em seguida, tentar novamente AD FS logon.  

## <a name="pre-requisites"></a>Pré-requisitos

Os pré-requisitos a seguir são necessários ao usar o Azure MFA para autenticação com o AD FS:  
  
- Uma [assinatura do Azure com Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/).  
- [Autenticação multifator do Azure](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/) 


> [!NOTE]
> O Azure AD e o Azure MFA estão incluídos no Azure AD Premium e no EMS (Enterprise Mobility Suite).  Se você tiver um deles, não precisará de assinaturas individuais.

- Um ambiente local do Windows Server 2016 AD FS.  
   - O servidor precisa ser capaz de se comunicar com as URLs a seguir pela porta 443.
      - https://adnotifications.windowsazure.com
      - https://login.microsoftonline.com
- Seu ambiente local é [federado com o Azure AD.](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Módulo do windows Azure Active Directory para Windows PowerShell](https://docs.microsoft.com/powershell/module/Azuread/?view=azureadps-2.0).  
- Permissões de administrador global em sua instância do Azure AD para configurá-lo usando o PowerShell do Azure AD.  
- Credenciais de administrador corporativo para configurar o farm de AD FS para o Azure MFA.

## <a name="configure-the-ad-fs-servers"></a>Configurar os servidores de AD FS

Para concluir a configuração do Azure MFA para AD FS, você precisa configurar cada servidor AD FS usando as etapas descritas. 

>[!NOTE]
>Certifique-se de que essas etapas sejam executadas em **todos os** servidores de AD FS no farm. Se você tiver vários servidores AD FS em seu farm, poderá executar a configuração necessária remotamente usando o PowerShell do Azure AD.  

### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Etapa 1: gerar um certificado para o Azure MFA em cada servidor de AD FS usando o cmdlet `New-AdfsAzureMfaTenantCertificate`

A primeira coisa que você precisa fazer é gerar um certificado para o Azure MFA usar.  Isso pode ser feito usando o PowerShell.  O certificado gerado pode ser encontrado no repositório de certificados de máquinas locais e é marcado com um nome de entidade que contém a Tenantid para seu diretório do Azure AD.

![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  

Observe que Tenantid é o nome do seu diretório no Azure AD.  Use o seguinte cmdlet do PowerShell para gerar o novo certificado.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  

![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-the-azure-multi-factor-auth-client-service-principal"></a>Etapa 2: adicionar as novas credenciais à entidade de serviço do cliente de autenticação multifator do Azure

Para permitir que os servidores de AD FS se comuniquem com o cliente de autenticação multifator do Azure, você precisa adicionar as credenciais à entidade de serviço para o cliente de autenticação multifator do Azure. Os certificados gerados usando o cmdlet `New-AdfsAzureMFaTenantCertificate` servirão como essas credenciais. Faça o seguinte usando o PowerShell para adicionar as novas credenciais à entidade de serviço de cliente da autenticação multifator do Azure.  

> [!NOTE]
> Para concluir esta etapa, você precisa se conectar à sua instância do Azure AD com o PowerShell usando `Connect-MsolService`.  Estas etapas pressupõem que você já se conectou por meio do PowerShell.  Para obter informações, consulte [`Connect-MsolService`.](https://msdn.microsoft.com/library/dn194123.aspx)  

**Definir o certificado como a nova credencial no cliente de autenticação multifator do Azure**  

`New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64`

> [!IMPORTANT]
> Esse comando precisa ser executado em todos os servidores de AD FS em seu farm.  A MFA do Azure AD falhará em servidores que não têm o certificado definido como a nova credencial no cliente de autenticação multifator do Azure.

> [!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 é o GUID do cliente de autenticação multifator do Azure.  
  
## <a name="configure-the-ad-fs-farm"></a>Configurar o farm de AD FS  
  
Depois de concluir a seção anterior em cada servidor de AD FS, defina as informações de locatário do Azure usando o cmdlet [set-AdfsAzureMfaTenant](https://docs.microsoft.com/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) . Esse cmdlet precisa ser executado apenas uma vez para um farm de AD FS.

Abra um prompt do PowerShell e insira seu próprio *tenantid* com o cmdlet [set-AdfsAzureMfaTenant](https://docs.microsoft.com/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) . Para clientes que usam Microsoft Azure Governamental nuvem, adicione o parâmetro `-Environment USGov`:

> [!NOTE]
> Você precisa reiniciar o serviço de AD FS em cada servidor no farm antes que essas alterações tenham efeito. Para um impacto mínimo, leve cada servidor de AD FS da rotação NLB, um de cada vez, e aguarde a descarga de todas as conexões.

```powershell
Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720
```

![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)

O Windows Server sem o service pack mais recente não oferece suporte ao parâmetro `-Environment` para o cmdlet [set-AdfsAzureMfaTenant](https://docs.microsoft.com/powershell/module/adfs/export-adfsauthenticationproviderconfigurationdata) . Se você usar a nuvem do Azure governamental e as etapas anteriores falharem ao configurar seu locatário do Azure devido ao parâmetro `-Environment` ausente, conclua as etapas a seguir para criar manualmente as entradas do registro. Ignore estas etapas se o cmdlet anterior registrou corretamente suas informações de locatário ou se você não estiver na nuvem do Azure governamental:

1. Abra o **Editor do registro** no servidor de AD FS.
1. Navegue até `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\ADFS`. Crie os seguintes valores de chave do registro:

    | Chave do Registro       | {1&gt;Valor&lt;1} |
    |--------------------|-----------------------------------|
    | SasUrl             | https://adnotifications.windowsazure.us/StrongAuthenticationService.svc/Connector |
    | StsUrl             | https://login.microsoftonline.us |
    | ResourceUri        | https://adnotifications.windowsazure.us/StrongAuthenticationService.svc/Connector |

1. Reinicie o serviço de AD FS em cada servidor no farm antes que essas alterações tenham efeito. Para um impacto mínimo, leve cada servidor de AD FS da rotação NLB, um de cada vez, e aguarde a descarga de todas as conexões.

Depois disso, você verá que o Azure MFA está disponível como um método de autenticação principal para uso de intranet e extranet.

![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Renovar e gerenciar AD FS certificados do Azure MFA

As orientações a seguir orientam você sobre como gerenciar os certificados do Azure MFA em seus servidores de AD FS.
Por padrão, quando você configura AD FS com o Azure MFA, os certificados gerados por meio do cmdlet `New-AdfsAzureMfaTenantCertificate` do PowerShell são válidos por 2 anos.  Para determinar a expiração dos certificados e, em seguida, renovar e instalar novos certificados, use o procedimento a seguir.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Avaliar AD FS data de expiração do certificado do Azure MFA

Em cada servidor de AD FS, no computador local meu repositório, haverá um certificado autoassinado com &#34;ou = Microsoft AD FS Azure MFA&#34; no emissor e no assunto.  Este é o certificado do Azure MFA.  Verifique o período de validade deste certificado em cada servidor de AD FS para determinar a data de validade.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Criar novo AD FS certificado do Azure MFA em cada servidor AD FS

Se o período de validade de seus certificados estiver se aproximando do seu fim, inicie o processo de renovação gerando um novo certificado de MFA do Azure em cada servidor de AD FS. Em uma janela de comando do PowerShell, gere um novo certificado em cada servidor de AD FS usando o seguinte cmdlet:

> [!CAUTION]
> Se o certificado já tiver expirado, não adicione o parâmetro `-Renew $true` ao comando a seguir. Nesse cenário, o certificado expirado existente é substituído por um novo em vez de ser deixado no local e um certificado adicional criado.

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

Se o certificado ainda não tiver expirado, será gerado um novo certificado válido de 2 dias no futuro para 2 dias + 2 anos. As operações AD FS e MFA do Azure não são afetadas por este cmdlet ou o novo certificado. (Observação: o atraso de 2 dias é intencional e fornece tempo para executar as etapas abaixo para configurar o novo certificado no locatário antes que AD FS comece a usá-lo para o Azure MFA.)

### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configurar cada novo AD FS certificado do Azure MFA no locatário do Azure AD

Usando o módulo do PowerShell do Azure AD, para cada novo certificado (em cada servidor de AD FS), atualize suas configurações de locatário do Azure AD da seguinte maneira (Observação: você deve primeiro se conectar ao locatário usando `Connect-MsolService` para executar os comandos a seguir).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $newcert
```

Se o certificado anterior já tiver expirado, reinicie o serviço de AD FS para selecionar o novo certificado. Você não precisa reiniciar o serviço de AD FS se você renovou um certificado antes que ele tenha expirado.

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Verifique se os novos certificados serão usados para o Azure MFA

Depois que os novos certificados se tornarem válidos, AD FS irá selecioná-los e começar a usar cada certificado respectivo para o Azure MFA dentro de algumas horas a um dia.  Quando isso ocorrer, em cada servidor, você verá um evento registrado no log de eventos AD FS admin com as seguintes informações:

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

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personalizar a página da Web AD FS para orientar os usuários a registrar os métodos de verificação de MFA

Use os exemplos a seguir para personalizar suas AD FS páginas da Web para usuários que ainda não foram reprovados (informações de verificação de MFA configuradas).

### <a name="find-the-error"></a>Localizar o erro

Primeiro, há algumas mensagens de erro diferentes AD FS retornará no caso em que o usuário não tem informações de verificação.
Se você estiver usando o Azure MFA como autenticação primária, o usuário não provado verá uma página de erro AD FS contendo as seguintes mensagens:
```
    <div id="errorArea">
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred
        </div>
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information.
        </div>
```
Quando o Azure AD, conforme a autenticação adicional estiver sendo tentada, o usuário não provado verá uma página de erro AD FS contendo as seguintes mensagens:
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

Para capturar o erro e mostrar as diretrizes personalizadas do usuário, basta acrescentar o JavaScript ao final do arquivo OnLoad. js que faz parte do tema da Web do AD FS.  Isso permite que você faça o seguinte:
 - Pesquisar as cadeias de caracteres de erro de identificação
 - forneça conteúdo da Web personalizado.  

> [!NOTE]
> Para obter orientação em geral sobre como personalizar o arquivo OnLoad. js, consulte o artigo [personalização avançada de AD FS páginas de entrada](advanced-customization-of-ad-fs-sign-in-pages.md).

Aqui está um exemplo simples, talvez você queira estender:

1. Abra o Windows PowerShell no seu servidor de AD FS primário e crie um novo AD FS tema da Web executando o seguinte comando:
    
    ``` PowerShell
        New-AdfsWebTheme –Name ProofUp –SourceName default
    ``` 
2. Em seguida, crie a pasta e exporte o tema da Web do AD FS padrão:

    ``` PowerShell
       New-Item -Path 'c:\Theme' -ItemType Directory;Export-AdfsWebTheme –Name default –DirectoryPath c:\Theme
    ```
3. Abra o arquivo C:\Theme\script\onload.js em um editor de texto
4. Acrescente o código a seguir ao final do arquivo OnLoad. js
    
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
    > Você precisa alterar "< YOUR_DOMAIN_NAME_HERE >"; para usar seu nome de domínio. Por exemplo: `var domain_hint = "contoso.com";`
    
5. Salve o arquivo OnLoad. js
6. Importe o arquivo OnLoad. js para seu tema personalizado digitando o seguinte comando do Windows PowerShell:
    
    ``` PowerShell
    Set-AdfsWebTheme -TargetName ProofUp -AdditionalFileResource @{Uri='/adfs/portal/script/onload.js';path="c:\theme\script\onload.js"}
    ```
7. Por fim, aplique o tema da Web do AD FS personalizado digitando o seguinte comando do Windows PowerShell:
    
    ``` PowerShell
    Set-AdfsWebConfig -ActiveThemeName "ProofUp"
    ```

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}

[Gerenciar protocolos TLS/SSL e conjuntos de codificação usados pelo AD FS e pelo Azure MFA](manage-ssl-protocols-in-ad-fs.md)
