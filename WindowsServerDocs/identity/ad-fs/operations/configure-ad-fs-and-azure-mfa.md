---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configurar o AD FS 2016 e MFA Azure
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/01/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be8e88ae36f344f1265fb76e66c19e0ac8aeb533
ms.sourcegitcommit: ffdfbb76c7f775e20d089d3f662d4f9a7d642f1e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/05/2018
---
# <a name="configure-ad-fs-2016-and-azure-mfa"></a>Configurar o AD FS 2016 e MFA Azure

>Aplica-se a: Windows Server 2016

Se sua organização for federada com o Azure AD, você pode usar o Azure a autenticação multifator para recursos de segurança do AD FS, ambos os locais e na nuvem. MFA Azure permite eliminar as senhas e fornecem uma maneira mais segura para autenticar.  A partir do Windows Server 2016, agora você pode configurar MFA do Azure para autenticação principal. 
  
Ao contrário com o AD FS no Windows Server 2012 R2, o adaptador MFA Azure AD FS 2016 integra-se diretamente com o Azure AD e não requer um servidor de MFA do Azure no local.   O adaptador do Azure MFA é integrado ao Windows Server 2016 e não é necessário para a instalação adicional.

## <a name="registering-users-for-azure-mfa-with-ad-fs-2016"></a>Registre-se os usuários de MFA do Azure com o AD FS 2016
AD FS não oferece suporte embutido "prova para cima" ou registro de informações de verificação de segurança do Azure MFA, como número de telefone ou aplicativo móvel. Isso significa que os usuários devem obter à prova de backup visitando https://account.activedirectory.windowsazure.com/Proofup.aspx antes de usar o Azure MFA para se autenticar em aplicativos do AD FS. Quando um usuário que tem não ainda à prova de backup em tentativas do Azure AD para se autenticar com o Azure MFA no AD FS, elas receberão um erro do AD FS.  Como um administrador do AD FS, você pode personalizar essa experiência de erro para orientar o usuário para a página proofup em vez disso.  Você pode fazer isso usando onload.js personalização para detectar a cadeia de caracteres de mensagem de erro dentro da página do AD FS e mostrar uma nova mensagem para orientar os usuários para visitar https://aka.ms/mfasetup e tentar novamente a autenticação. Para obter orientações detalhadas, consulte "Personalizar o AD FS página da web para orientar os usuários para registrar os métodos de verificação de MFA" abaixo neste artigo.

>[!NOTE]
> Anteriormente, os usuários precisavam se autenticar com MFA para registro (visitando https://account.activedirectory.windowsazure.com/Proofup.aspx, por exemplo, via o atalho aka.ms/mfasetup).  Agora, um usuário do AD FS que ainda não foram registrados informações de verificação de MFA pode acessar o Azure AD da página proofup via o atalho aka.ms/mfasetup usando autenticação apenas principal (como páginas da web de autenticação integrada do Windows ou nome de usuário e senha por meio do AD FS) .  Se o usuário não tem métodos nenhuma verificação configurados, Azure AD executará registro embutido em que o usuário vê a mensagem "o administrador é necessário configurar essa conta para verificação de segurança adicionais", e o usuário pode selecionar "Configurá-la agora".
> Os usuários que já têm pelo menos um método de verificação de MFA configurado ainda precisará fornecer MFA ao visitar a página proofup.

### <a name="recommended-deployment-topologies"></a>Topologias de implantação recomendada

#### <a name="azure-mfa-as-primary-authentication"></a>Azure MFA como autenticação principal
Há alguns motivos para usar a MFA do Azure como principal autenticação com o AD FS:
 - Para evitar senhas para entrar no Azure AD, Office 365 e outros aplicativos do AD FS
 - Para proteger a senha com base em entrada exigindo um fator adicional, como código de verificação antes da senha

Se você quiser usar a MFA do Azure como um método de autenticação primária no AD FS para atingir esses benefícios, você provavelmente também queira manter a capacidade de usar o Azure AD condicional acesso incluindo "true MFA" pedindo para fatores adicionais no AD FS.

Agora você pode fazer isso definindo a configuração de domínio do Azure AD para fazer MFA no local (configuração "SupportsMfa" para $True).  Nesta configuração, AD FS pode ser solicitado pelo Azure AD para realizar autenticação adicional ou "MFA true" para cenários de acesso condicional que precisam dele.  

Conforme descrito anteriormente, qualquer usuário do AD FS que ainda não (configurado MFA verificação informações registradas) devem ser solicitadas por meio de uma página de erro personalizada do AD FS visitar aka.ms/mfasetup para configurar as informações de verificação, novamente tente logon do AD FS.  
Porque MFA Azure como principal é considerado um fator único, depois que os usuários de configuração inicial serão necessário fornecer um fator adicional para gerenciar ou atualizar suas informações de verificação no Azure AD, ou para acessar outros recursos que exigem MFA.


#### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Azure MFA como autenticação adicional para o Office 365
Anteriormente, se você quisesse tem Azure MFA como um método de autenticação adicional no AD FS para Office 365 ou outras partes confiantes, a melhor opção configurar o Azure AD para composta MFA, em que a autenticação principal é executada no local no AD FS e MFA é tr iggered pelo Azure AD. Agora, você pode usar o Azure MFA como autenticação adicional no AD FS quando a configuração de SupportsMfa domínio está definida como $True.  

Conforme descrito anteriormente, qualquer usuário do AD FS que ainda não (configurado MFA verificação informações registradas) devem ser solicitadas por meio de uma página de erro personalizada do AD FS visitar aka.ms/mfasetup para configurar as informações de verificação, novamente tente logon do AD FS.  


## <a name="pre-requisites"></a>Pré-requisitos  
Pré-requisitos a seguir são necessários ao usar o Azure MFA para autenticação com o AD FS:  
  
- Um [assinatura do Azure com o Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/).  
- [Autenticação multifator Azure](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/)  

>[!NOTE]   
> Azure AD e MFA do Azure estão incluídos no Azure AD Premium e o Enterprise Mobility Suite (EMS).  Se você tiver um destes procedimentos, você não precise assinaturas individuais.   
- Um ambiente do Windows Server 2016 AD FS no local.  
- Seu ambiente local é [federados com o Azure AD.](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Módulo do Active Directory do Azure Windows para Windows PowerShell](https://go.microsoft.com/fwlink/p/?linkid=236297).  
- Permissões de administrador global na sua instância do Azure AD para configurá-lo usando o Azure AD PowerShell.  
- Credenciais de administrador corporativo para configurar o farm AD FS mfa do Azure.  
  
  
## <a name="configure-the-ad-fs-servers"></a>Configure os servidores do AD FS  
Para concluir a configuração para Azure MFA do AD FS, você precisa configurar cada servidor AD FS usando as etapas descritas. 

>[!NOTE]
>Certifique-se de que essas etapas sejam executadas em **todos os** servidores do AD FS no farm. Se tiver vários servidores do AD FS no seu farm, você pode realizar a configuração necessária remotamente usando o Azure AD Powershell.  
  
  
  
### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Etapa 1: Gerar um certificado para Azure MFA em cada servidor AD FS usando o `New-AdfsAzureMfaTenantCertificate`cmdlet.   
A primeira coisa que você precisa fazer é gerar um certificado para o Azure MFA usar.  Isso pode ser feito usando o PowerShell.  O certificado gerado pode ser encontrado no repositório de certificados de computadores locais e ele é marcado com um nome de entidade que contém o TenantID para o diretório do Azure AD.    
  
![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  
  
Observe que TenantID é o nome da pasta no Azure AD.  Use o seguinte cmdlet do PowerShell para gerar o novo certificado.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  
      
![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-azure-multi-factor-auth-client-spn"></a>Etapa 2: Adicionar as novas credenciais para Azure multifator Auth cliente SPN   
Para habilitar os servidores do AD FS para se comunicar com o cliente de autenticação multifator Azure, você precisa adicionar as credenciais para o SPN para o cliente de autenticação multifator Azure. Os certificados gerados usando a `New-AdfsAzureMFaTenantCertificate`cmdlet servirá essas credenciais. Faça o seguinte usando o PowerShell para adicionar as novas credenciais para o SPN de cliente do Azure multifator Auth.  

>[!NOTE]   
>Para concluir essa etapa, você precisa se conectar à sua instância do Azure AD com o PowerShell usando Connect-MsolService.  Estas etapas pressupõem que você já tenha se conectado por meio do PowerShell.  Para obter informações, consulte [MsolService conectar.](https://msdn.microsoft.com/library/dn194123.aspx)  
     
  
2. **Defina o certificado como a nova credencial contra o cliente de autenticação multifator Azure**  
    
    `New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64 `

>[!IMPORTANT]
>  Esse comando precisa ser executado em todos os servidores do AD FS no seu farm.  Azure AD MFA falhará em servidores que não tem o certificado definido como a nova credencial contra o cliente de autenticação multifator Azure. 

>[!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 é o guid para o cliente de autenticação multifator Azure.  
  
## <a name="configure-the-ad-fs-farm"></a>Configurar o AD FS Farm  
  
Depois de ter concluído a seção anterior em cada servidor AD FS, você precisará executar o `Set-AdfsAzureMfaTenant`cmdlet.  
  
Este cmdlet precisa ser executado apenas uma vez para um farm AD FS.  Use o PowerShell para concluir essa etapa.    
  
>[!NOTE]  
>Você precisará reiniciar o serviço do AD FS em cada servidor no farm antes que essas alterações entrem em vigor.  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  
      
![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
Depois disso, você verá que MFA Azure está disponível como um método de autenticação primária de intranet e extranet uso.    
  
![AD FS e MFA](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Renovar e gerenciar o AD FS Azure MFA certificados
A orientação a seguir leva você por meio de como gerenciar os certificados de MFA do Azure em seus servidores do AD FS.
Por padrão, quando você configura o AD FS com MFA do Azure, os certificados gerados via o cmdlet do PowerShell New-AdfsAzureMfaTenantCertificate são válidos por 2 anos.  Para determinar como fechar para expiração os certificados são e, em seguida, para renovar e instalar novos certificados, use o procedimento a seguir.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Avaliar a data de validade do certificado AD FS Azure MFA
Em cada servidor AD FS, no computador local meu repositório, haverá um certificado auto-assinado com "UO = Microsoft AD FS Azure MFA" no assunto e emissor.  Este é o certificado de MFA do Azure.  Verifique o período de validade desse certificado em cada servidor AD FS para determinar a data de validade.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Criar novo AD FS Azure MFA certificado em cada servidor AD FS
Se o período de validade de seus certificados está se aproximando a fim, inicie o processo de renovação, gerando um novo certificado Azure MFA em cada servidor AD FS. Em uma janela de comando do powershell, gere um novo certificado em cada servidor AD FS usando o seguinte cmdlet:

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

Como resultado este cmdlet, será gerado um novo certificado é válido de 2 dias no futuro para 2 dias + 2 anos.  Operações do AD FS e Azure MFA não serão afetadas por este cmdlet ou o novo certificado. (Observação: o atraso de 2 dias é intencional e fornece tempo para executar as etapas abaixo para configurar o novo certificado no locatário antes do AD FS é iniciado usando o Azure mfa.)


### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configurar cada novo certificado AD FS Azure MFA no locatário do Azure AD
Como usar o módulo do PowerShell do Azure AD, para cada novo certificado (em cada servidor AD FS), atualize suas configurações de locatário do Azure AD da seguinte forma (Observação: você deve primeiro conectar o locatário usando Connect-MsolService para executar os comandos a seguir).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $certbase64
```
    Where $certbase64 is the new certificate.  The base64 encoded certificate can be obtained by exporting the certificate (without the private key) as a DER encoded file and opening in Notepad.exe, then copy/pasting to the PSH session and assigning to the variable $certbase64

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Verifique se que os novos certificados serão usados para MFA do Azure
Uma vez os novos certificados se tornam válidos, AD FS coletá-las e começar a usar cada certificado respectivo mfa do Azure dentro de algumas horas para um dia.  Assim que isso ocorre, em cada servidor você verá um evento registrado no log de eventos do administrador do AD FS com as seguintes informações: nome do Log: AD FS/administrador fonte: AD FS data: 27/2/2018 ID de evento de 7:33:31 PM: 547 categoria da tarefa: nenhum nível: palavras-chave de informações : AD FS usuário: DOMAIN\adfssvc computador: ADFS.domain.contoso.com Descrição: O certificado de locatário Azure mfa foi renovado.  

TenantId: contoso.onmicrosoft.com. Impressão digital do antigo: 7CC103D60967318A11D8C51C289EF85214D9FC63. Data de expiração antiga: 15/9/2019 9:43:17 PM. Impressão digital do novo: 8110D7415744C9D4D5A4A6309499F7B48B5F3CCF. Nova data de validade: 27/2/2020 2:16:07 AM.

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personalizar a página da web do AD FS para orientar os usuários para registrar os métodos de verificação de MFA

Use os exemplos a seguir personalizar suas páginas da web do AD FS para usuários que ainda não tiverem à prova para cima (configurado informações de verificação de MFA).

### <a name="find-the-error"></a>Encontrar o erro
Primeiro, há algumas das mensagens de erro diferentes que AD FS retornará no caso em que o usuário não tem informações de verificação.
Se você estiver usando o Azure MFA como autenticação principal, o usuário cancelou proofed será exibida uma página de erro do AD FS que contém as mensagens a seguir:
```
    <div id="errorArea"> 
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred 
        </div> 
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information. 
        </div>
```
Quando está sendo tentado do Azure AD como autenticação adicional, o usuário cancelou proofed será exibida uma página de erro do AD FS que contém as mensagens a seguir:
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
Capturar o erro e mostrar ao usuário personalizado diretriz é uma questão de acrescentando javascript até o final do arquivo onload.js que faz parte do AD FS web tema para (1) procurar as cadeias de caracteres de erro identificação e (2) fornecem personalizado conteúdo da Web.  (Para obter orientação em geral sobre como personalizar o arquivo onload.js, consulte o artigo [aqui](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/advanced-customization-of-ad-fs-sign-in-pages).)

