---
title: Criar um método de autenticação personalizado para AD FS no Windows Server
description: Este cenário descreve como criar um método de autenticação personalizado para AD FS no Windows Server.
author: billmath
ms.author: billmath
manager: daveba
ms.date: 05/23/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: a5cff8ea1a7792906e5fd74981772e4760a2808c
ms.sourcegitcommit: 8eea7aadbe94f5d4635c4ffedc6a831558733cc0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66314708"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>Criar um método de autenticação personalizado para AD FS no Windows Server

Este passo a passo fornece instruções para a implementação de um método de autenticação personalizado para AD FS no Windows Server 2012 R2. Para obter mais informações, consulte [métodos de autenticação adicionais](https://msdn.microsoft.com/en-us/library/dn758113\(v=msdn.10\)).


> [!WARNING]
> O exemplo que você pode criar aqui é&nbsp;apenas para fins educacionais. &nbsp;Essas instruções são para a implementação mais simples e mais concisa possível expor os elementos necessários do modelo.&nbsp; Não há nenhuma autenticação back-end, processamento de erro ou dados de configuração. 
> <P></P>



## <a name="setting-up-the-development-box"></a>Como configurar a caixa de desenvolvimento

Este passo a passo usa o Visual Studio 2012.  O projeto pode ser compilado usando qualquer ambiente de desenvolvimento que pode criar uma classe do .NET para Windows. O projeto deve ter como destino .NET 4.5 porque o **BeginAuthentication** e **TryEndAuthentication** métodos usam o tipo **System.Security.Claims.Claim**, que faz parte do .NET 4.5.There de versão do Framework é uma referência necessária para o projeto:


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>Dll de referência</strong></p></td>
<td><p><strong>Onde encontrá-lo</strong></p></td>
<td><p><strong>Necessário para</strong></p></td>
</tr>
<tr class="even">
<td><p>Microsoft.IdentityServer.Web.dll</p></td>
<td><p>A dll está localizada em %windir%\ADFS em um servidor Windows Server 2012 R2 no qual o AD FS foi instalado.</p>
<p></p>
<p>Essa dll deve ser copiado para a máquina de desenvolvimento e uma referência explícita criados no projeto.</p></td>
<td><p>Tipos de interface incluindo IAuthenticationContext, IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>Crie o provedor

1.  No Visual Studio 2012: Escolha arquivo -\>New -\>projeto...

2.  Selecione a biblioteca de classes e certifique-se de que você estiver direcionando o .NET 4.5.
    
    ![Crie o provedor](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "criar o provedor")

3.  Faça uma cópia do **Microsoft.IdentityServer.Web.dll** da pasta % windir %\\ADFS no servidor Windows Server 2012 R2 em que o AD FS foi instalado e cole-o na pasta do projeto em seu computador de desenvolvimento.

4.  Na **Gerenciador de soluções**, clique com botão direito **referências** e **adicionar referência...**

5.  Navegue até sua cópia local do **Microsoft.IdentityServer.Web.dll** e **adicionar...**

6.  Clique em **Okey** para confirmar a nova referência:
    
    ![Crie o provedor](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "criar o provedor")
    
    Você deve agora ser configurada para resolver todos os tipos necessários para o provedor. 

7.  Adicione uma nova classe ao seu projeto (clique com botão direito no seu projeto, **adicionar... Classe...** ) e dê a ele um nome como **MyAdapter**, conforme mostrado abaixo:
    
    ![Crie o provedor](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "criar o provedor")

8.  No novo arquivo MyAdapter.cs, substitua o código existente pelo seguinte:
    
        using System;
         using System.Collections.Generic;
         using System.Linq;
         using System.Text;
         using System.Threading.Tasks;
         using System.Globalization;
         using System.IO;
         using System.Net;
         using System.Xml.Serialization;
         using Microsoft.IdentityServer.Web.Authentication.External;
         using Claim = System.Security.Claims.Claim;
         
         namespace MFAadapter
         {
         class MyAdapter : IAuthenticationAdapter
         {
         
         }
         }
    
    Agora você deve ser capaz de F12 (clique com botão direito – ir para definição) na IAuthenticationAdapter para ver o conjunto de membros de interface necessária. 
    
    Em seguida, você pode fazer uma simple implementação deles.

9.  Substitua todo o conteúdo da sua classe com o seguinte:
    
        namespace MFAadapter
         {
         class MyAdapter : IAuthenticationAdapter
         {
         public IAuthenticationAdapterMetadata Metadata
         {
         //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
         }
         
         public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
         {
         //return new instance of IAdapterPresentationForm derived class
         
         }
         
         public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
         {
         return true; //its all available for now
         
         }
         
         public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
         {
         //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter
         
         }
         
         public void OnAuthenticationPipelineUnload()
         {
         
         }
         
         public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
         {
         //return new instance of IAdapterPresentationForm derived class
         
         }
         
         public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
         {
         //return new instance of IAdapterPresentationForm derived class
         
         }
         
         }
         }

10. Não estamos prontos para compilar ainda... Existem duas interfaces mais ir.
    
    Adicione duas classes mais ao seu projeto: um é para os metadados e o outro para o formato de apresentação.  Você pode adicionar esses dentro do mesmo arquivo como a classe acima.
    
        class MyMetadata : IAuthenticationAdapterMetadata
         {
         
         }
         
         class MyPresentationForm : IAdapterPresentationForm
         {
         
         }

11. Em seguida, você pode adicionar os membros necessários para cada um. Primeiro, os metadados (com comentários úteis embutido)
    
        class MyMetadata : IAuthenticationAdapterMetadata
         {
         //Returns the name of the provider that will be shown in the AD FS management UI (not visible to end users)
         public string AdminName
         {
         get { return "My Example MFA Adapter"; }
         }
         
         //Returns an array of strings containing URIs indicating the set of authentication methods implemented by the adapter 
         /// AD FS requires that, if authentication is successful, the method actually employed will be returned by the
         /// final call to TryEndAuthentication(). If no authentication method is returned, or the method returned is not
         /// one of the methods listed in this property, the authentication attempt will fail.
         public virtual string[] AuthenticationMethods 
         {
         get { return new[] { "http://example.com/myauthenticationmethod1", "http://example.com/myauthenticationmethod2" }; }
         }
         
         /// Returns an array indicating which languages are supported by the provider. AD FS uses this information
         /// to determine the best language\locale to display to the user.
         public int[] AvailableLcids
         {
         get
         {
         return new[] { new CultureInfo("en-us").LCID, new CultureInfo("fr").LCID};
         }
         }
         
         /// Returns a Dictionary containing the set of localized friendly names of the provider, indexed by lcid. 
         /// These Friendly Names are displayed in the "choice page" offered to the user when there is more than 
         /// one secondary authentication provider available.
         public Dictionary<int, string> FriendlyNames
         {
         get
         {
         Dictionary<int, string> _friendlyNames = new Dictionary<int, string>();
         _friendlyNames.Add(new CultureInfo("en-us").LCID, "Friendly name of My Example MFA Adapter for end users (en)");
         _friendlyNames.Add(new CultureInfo("fr").LCID, "Friendly name translated to fr locale");
         return _friendlyNames;
         }
         }
         
         /// Returns a Dictionary containing the set of localized descriptions (hover over help) of the provider, indexed by lcid. 
         /// These descriptions are displayed in the "choice page" offered to the user when there is more than one 
         /// secondary authentication provider available.
         public Dictionary<int, string> Descriptions
         {
         get 
         {
         Dictionary<int, string> _descriptions = new Dictionary<int, string>();
         _descriptions.Add(new CultureInfo("en-us").LCID, "Description of My Example MFA Adapter for end users (en)");
         _descriptions.Add(new CultureInfo("fr").LCID, "Description translated to fr locale");
         return _descriptions; 
         }
         }
         
         /// Returns an array indicating the type of claim that that the adapter uses to identify the user being authenticated.
         /// Note that although the property is an array, only the first element is currently used.
         /// MUST BE ONE OF THE FOLLOWING
         /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/windowsaccountname"
         /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn"
         /// "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/emailaddress"
         /// "http://schemas.microsoft.com/ws/2008/06/identity/claims/primarysid"
         public string[] IdentityClaims
         {
         get { return new[] { "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" }; }
         }
         
         //All external providers must return a value of "true" for this property.
         public bool RequiresIdentity
         {
         get { return true; }
         }
        }
    
    Em seguida, o formato de apresentação:
    
        class MyPresentationForm : IAdapterPresentationForm
         {
         /// Returns the HTML Form fragment that contains the adapter user interface. This data will be included in the web page that is presented
         /// to the cient.
         public string GetFormHtml(int lcid)
         {
         string htmlTemplate = Resources.FormPageHtml; //todo we will implement this
         return htmlTemplate;
         }
         
         /// Return any external resources, ie references to libraries etc., that should be included in 
         /// the HEAD section of the presentation form html. 
         public string GetFormPreRenderHtml(int lcid)
         {
         return null;
         }
         
         //returns the title string for the web page which presents the HTML form content to the end user
         public string GetPageTitle(int lcid)
         {
         return "MFA Adapter";
         }
         
         
         }

12. Observe o todo para o **Resources.FormPageHtml** acima do elemento. 
    
    Você pode corrigi-lo em um minuto, mas primeiro vamos adicionar as finais necessárias instruções de retorno, com base em tipos implementados recentemente, a sua classe MyAdapter inicial.  Para fazer isso, adicione os itens na *itálico* abaixo para a implementação de IAuthenticationAdapter existente:
    
        class MyAdapter : IAuthenticationAdapter
         {
         public IAuthenticationAdapterMetadata Metadata
         {
         //get { return new <instance of IAuthenticationAdapterMetadata derived class>; }
         get { return new MyMetadata(); }
         }
         
         public IAdapterPresentation BeginAuthentication(Claim identityClaim, HttpListenerRequest request, IAuthenticationContext authContext)
         {
         //return new instance of IAdapterPresentationForm derived class
         return new MyPresentationForm();
         }
         
         public bool IsAvailableForUser(Claim identityClaim, IAuthenticationContext authContext)
         {
         return true; //its all available for now
         }
         
         public void OnAuthenticationPipelineLoad(IAuthenticationMethodConfigData configData)
         {
         //this is where AD FS passes us the config data, if such data was supplied at registration of the adapter
         
         }
         
         public void OnAuthenticationPipelineUnload()
         {
         
         }
         
         public IAdapterPresentation OnError(HttpListenerRequest request, ExternalAuthenticationException ex)
         {
         //return new instance of IAdapterPresentationForm derived class
         return new MyPresentationForm();
         }
         
         public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
         {
         //return new instance of IAdapterPresentationForm derived class
         outgoingClaims = new Claim[0];
         return new MyPresentationForm();
         }
         
         }

13. Agora para o arquivo de recurso que contém o fragmento de html. Crie um novo arquivo de texto na pasta do projeto com o seguinte conteúdo:
    
        <div id="loginArea">
         <form method="post" id="loginForm" >
         <!-- These inputs are required by the presentation framework. Do not modify or remove -->
         <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
         <input id="context" type="hidden" name="Context" value="%Context%"/>
         <!-- End inputs are required by the presentation framework. -->
         <p id="pageIntroductionText">This content is provided by the MFA sample adapter. Challenge inputs should be presented below.</p>
         <label for="challengeQuestionInput" class="block">Question text</label>
         <input id="challengeQuestionInput" name="ChallengeQuestionAnswer" type="text" value="" class="text" placeholder="Answer placeholder" />
         <div id="submissionArea" class="submitMargin">
         <input id="submitButton" type="submit" name="Submit" value="Submit" onclick="return AuthPage.submitAnswer()"/>
         </div>
         </form>
         <div id="intro" class="groupMargin">
         <p id="supportEmail">Support information</p>
         </div>
         <script type="text/javascript" language="JavaScript">
         //<![CDATA[
         function AuthPage() { }
         AuthPage.submitAnswer = function () { return true; };
         //]]>
         </script></div>

14. Em seguida, selecione **projeto -\>Add Component... Recursos** do arquivo e nomeie o arquivo **recursos**e clique em **adicionar:**
    
    ![Crie o provedor](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "criar o provedor")

15. Em seguida, dentro de **Resources** do arquivo, escolha **adicionar recurso... Adicionar arquivo existente**.  Navegue até o arquivo de texto (que contém o fragmento de html) que você salvou acima.
    
    Certifique-se de que seu código GetFormHtml resolve o nome do novo recurso corretamente, o prefixo do nome de arquivo (. resx) recursos seguido do nome do recurso em si:
    
        public string GetFormHtml(int lcid)
        {
         string htmlTemplate = Resources.MfaFormHtml; //Resxfilename.resourcename
         return htmlTemplate;
        }
    
    Agora você deve ser capaz de criar.

## <a name="build-the-adapter"></a>Crie o adaptador

O adaptador deve ser compilado em um assembly .NET fortemente nomeado que pode ser instalado no GAC no Windows.  Para fazer isso em um projeto do Visual Studio, conclua as seguintes etapas:

1.  Clique com botão direito no nome do projeto no Gerenciador de soluções e clique em **propriedades**.

2.  Sobre o **Signing** guia, seleção **assinar o assembly** e escolha **\<New... \>** sob **escolher um arquivo de chave de nome forte:**   Insira um nome de arquivo de chave e uma senha e clique em **Okey**.  Em seguida, certifique-se **assinar o assembly** está marcada e **somente sinal de atraso** está desmarcada.  As propriedades **Signing** página deve ter esta aparência:
    
    ![criar o provedor](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "compilar o provedor")

3.  Em seguida, compile a solução.

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>Implantar o adaptador em seu computador de teste do AD FS

Antes de um provedor externo pode ser invocado pelo AD FS, ele deve ser registrado no sistema.  Provedores do adaptador devem fornecer um instalador que executa as ações de instalação necessários, incluindo a instalação no GAC e o instalador deve dar suporte a registro no AD FS.  Se isso não for concluído, o administrador precisa executar as etapas do Windows PowerShell abaixo.  Essas etapas podem ser usadas no laboratório para habilitar o teste e depuração.

### <a name="prepare-the-test-ad-fs-machine"></a>Preparar o computador de teste do AD FS

Copiar arquivos e adicionar ao GAC.

1.  Verifique se que você tiver um computador Windows Server 2012 R2 ou máquina virtual.

2.  Instalar o serviço de função do AD FS e configurar um farm pelo menos um nó.
    
    Para obter etapas detalhadas configurar um servidor de Federação em um ambiente de laboratório, consulte o [guia de implantação do Windows Server 2012 R2 AD FS](https://msdn.microsoft.com/en-us/library/dn486820\(v=msdn.10\)).

3.  Copie as ferramentas de Gacutil.exe para o servidor.
    
    Gacutil.exe pode ser encontrado na **% homedrive %\\arquivos de programas (x86)\\SDKs da Microsoft\\Windows\\v8.0A\\bin\\NETFX 4.0 Tools\\** em um computador Windows 8.  Será necessário o **gacutil.exe** próprio arquivo, bem como a **1033**, **en-US**e a outra pasta de recurso localizado abaixo de **NETFX 4.0 Tools** local.

4.  Copie os arquivos de provedor (assinados de nome forte de um ou mais arquivos. dll) para o mesmo local da pasta **gacutil.exe** (o local é apenas para conveniência)

5.  Adicione os arquivos. dll no GAC em cada servidor de Federação do AD FS no farm:
    
    Exemplo: usando a ferramenta de linha de comando GACutil.exe para adicionar uma dll no GAC: `C:\>.\gacutil.exe /if .\<yourdllname>.dll`
    
    Para exibir a entrada resultante no GAC:`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>Registrar o provedor no AD FS

Depois que os pré-requisitos acima forem atendidos, abra uma janela de comando do Windows PowerShell no servidor de Federação e insira os comandos a seguir (Observe que, se você estiver usando um farm de servidores de federação que usa o banco de dados interno do Windows, você deve executar esses comandos no servidor de Federação primário do farm):

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`
    
    Onde YourTypeName é o nome de tipo forte do .NET: "YourDefaultNamespace.YourIAuthenticationAdapterImplementationClassName, YourAssemblyName, versão = YourAssemblyVersion, Culture = neutral, PublicKeyToken = YourPublicKeyTokenValue, processorArchitecture = MSIL"
    
    Isso registrará seu provedor externo no AD FS, com o nome que você forneceu como AnyNameYouWish acima.

2.  Reinicie o serviço do AD FS (usando o snap-in de serviços do Windows, por exemplo).

3.  Execute o seguinte comando: `Get-AdfsAuthenticationProvider`.
    
    Isso mostra o provedor como um dos provedores no sistema.
    
    Exemplo:
    
        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv
    
    Se você tiver o serviço de registro de dispositivo habilitado também em seu ambiente do AD FS, execute o seguinte:  `PS C:\>net start drs`
    
    Para verificar se o provedor registrado, use o seguinte comando:`PS C:\>Get-AdfsAuthenticationProvider`.
    
    Isso mostra o provedor como um dos provedores no sistema.

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>Criar a política de autenticação do AD FS que invoca o adaptador

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Criar a política de autenticação usando o snap-in de gerenciamento do AD FS

1.  Abra o snap-in de gerenciamento do AD FS (no Gerenciador de servidores **ferramentas** menu).

2.  Clique em **políticas de autenticação**.

3.  No painel central, sob **a autenticação multifator**, clique no **editar** link à direita de **configurações globais**.

4.  Sob **Selecionar métodos de autenticação adicionais** na parte inferior da página, marque a caixa para AdminName do seu provedor. Clique em **Aplicar**.

5.  Para fornecer um "gatilho" para invocar usando menos de seu adaptador MFA **locais** marque ambos **Extranet** e **Intranet**, por exemplo. Clique em **OK**. (Para configurar gatilhos por terceira parte confiável de terceiros, consulte "Criar a política de autenticação usando o Windows PowerShell" abaixo.)

6.  Verificar os resultados usando os comandos a seguir:
    
    Primeiro use `Get-AdfsGlobalAuthenticationPolicy`. Você deve ver o nome do provedor como um dos valores de AdditionalAuthenticationProvider.
    
    Em seguida, use `Get-AdfsAdditionalAuthenticationRule`. Você deve ver as regras para Extranet e Intranet configurados como resultado de sua seleção de política na interface do usuário do administrador.

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>Criar a política de autenticação usando o Windows PowerShell

1.  Primeiro, habilite o provedor de política global:
    
    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`
    

    > [!NOTE]
    > Observe que o valor fornecido para o parâmetro AdditionalAuthenticationProvider corresponde ao valor fornecido para o parâmetro "Name" no cmdlet Register-AdfsAuthenticationProvider acima e à propriedade "Name" do Saída do cmdlet Get-AdfsAuthenticationProvider. 
    > <P></P>

    
    Exemplo:`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`

2.  Em seguida, configure regras globais ou específicos de terceira parte confiável de terceiros para disparar a MFA:
    
    Exemplo 1: criar a regra global para exigir MFA para solicitações externas:`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`
    
    Exemplo 2: criar o MFA de regras para exigir MFA para as solicitações externas para uma terceira parte confiável específica terceiros.  (Observe que os provedores individuais não podem ser conectadas a terceiras partes confiáveis individuais no AD FS no Windows Server 2012 R2).
    
        PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
        PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>Autenticar com o MFA usando o adaptador

Por fim, execute as etapas abaixo para seu adaptador de teste:

1.  Verifique se o tipo de autenticação primária global do AD FS está configurado como autenticação de formulários para Extranet e Intranet (Isso facilita sua demonstração autenticar como um usuário específico)
    
    1.  No AD FS, snap-in, em **políticas de autenticação**, no **autenticação primária** área, clique em **editar** lado **configurações globais**.
        
        1.  Ou simplesmente clique a **primário** da guia a **política multifator** interface do usuário.

2.  Certifique-se **autenticação de formulários** é a única opção marcada para o método de autenticação de Intranet e Extranet.  Clique em **OK**.

3.  Página de logon html aberto iniciado pelo IDP (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) e entre como um usuário válido do AD em seu ambiente de teste.

4.  Insira as credenciais para autenticação primária.

5.  Você deve ver os formulários MFA a página com dúvidas de desafio de exemplo são exibidos. 
    
    Se você tiver mais de um adaptador configurado, você verá a página de opção MFA com o nome amigável acima.
    
    ![autenticar com adaptador](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "autenticar com adaptador")
    
    ![autenticar com adaptador](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "autenticar com adaptador")

Agora você tem uma implementação funcional da interface e você tem o conhecimento de como funciona o modelo. Você pode trym como um exemplo adicional para definir pontos de interrupção no BeginAuthentication, bem como o TryEndAuthentication.  Observe como BeginAuthentication é executado quando o usuário entra pela primeira vez o formulário MFA, enquanto TryEndAuthentication é disparada em cada envio do formulário.

## <a name="update-the-adapter-for-successful-authentication"></a>Atualizar o adaptador para uma autenticação bem-sucedida

Mas espera – exemplo de adaptador será nunca se autenticar com êxito\!  Isso ocorre porque nada em seu código retorna null para TryEndAuthentication.

Ao concluir os procedimentos acima, você criou uma implementação de adaptador básicas e ele foi adicionado a um servidor do AD FS.  Você pode obter a página de formulários MFA, mas você não ainda autenticado porque você tem ainda não colocam a lógica correta em sua implementação TryEndAuthentication.  Então, vamos adicioná-la.

Lembre-se de sua implementação TryEndAuthentication:

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();
     
     }

Vamos atualizá-lo para que ela nem sempre retorna MyPresentationForm().  Para isso, você pode criar um método de utilitário simples em sua classe:

    static bool ValidateProofData(IProofData proofData, IAuthenticationContext authContext)
     {
     if (proofData == null || proofData.Properties == null || !proofData.Properties.ContainsKey("ChallengeQuestionAnswer"))
     {
     throw new ExternalAuthenticationException("Error - no answer found", authContext);
     }
     
     if ((string)proofData.Properties["ChallengeQuestionAnswer"] == "adfabric")
     {
     return true;
     }
     else
     {
     return false;
     }
     }

Em seguida, atualize TryEndAuthentication conforme mostrado a seguir:

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     outgoingClaims = new Claim[0];
     if (ValidateProofData(proofData, authContext))
     {
     //authn complete - return authn method
     outgoingClaims = new[] 
     {
     // Return the required authentication method claim, indicating the particulate authentication method used.
     new Claim( "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", 
     "http://example.com/myauthenticationmethod1" )
     };
     return null;
     }
     else
     {
     //authentication not complete - return new instance of IAdapterPresentationForm derived class
     return new MyPresentationForm();
     }
     }

Agora você precisa atualizar o adaptador de caixa de teste.  Você deve primeiro desfazer a diretiva do AD FS, em seguida, cancelar o registro do AD FS e reinicie AD FS, e em seguida, remover o arquivo. dll no GAC, e em seguida, adicionar o novo arquivo. dll no GAC, e em seguida, registrá-lo no AD FS, reiniciar o AD FS e configurar novamente a política do AD FS.

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>Implantar e configurar o adaptador atualizado em seu teste de máquina do AD FS

### <a name="clear-ad-fs-policy"></a>Limpar política do AD FS

Desmarque MFA todas as relacionadas a caixas de seleção na UI MFA, mostrado abaixo, clique em Okey.

![Limpar política](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "limpar política")

### <a name="unregister-provider-windows-powershell"></a>Cancelar o registro do provedor (Windows PowerShell)

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

Exemplo:`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

Observe que o valor que você passa para "Name" é o mesmo valor como "Name" que você forneceu para o cmdlet Register-AdfsAuthenticationProvider.  Também é a propriedade "Name" que é a saída do Get-AdfsAuthenticationProvider.

Observe que, antes de você cancelar o registro de um provedor, você deve remover o provedor do AdfsGlobalAuthenticationPolicy (ou desmarcando as caixas de seleção marcada no snap-in de gerenciamento do AD FS ou usando o Windows PowerShell.)

Observe que o serviço AD FS deve ser reiniciado depois dessa operação.

### <a name="remove-assembly-from-gac"></a>Remover o assembly do GAC

1.  Primeiro, use o comando a seguir para localizar o nome forte totalmente qualificado da entrada:`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`
    
    Exemplo:`C:\>.\gacutil.exe /l mfaadapter`

2.  Em seguida, use o seguinte comando para removê-lo no GAC:`.\gacutil /u “<output from the above command>”`
    
    Exemplo:`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>Adicionar assembly atualizado ao GAC

Verifique se que você colar atualizado. dll localmente pela primeira vez. `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>Assembly de modo de exibição no GAC (linha de comando)

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>Registrar o provedor no AD FS

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  Reinicie o serviço do AD FS.

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Criar a política de autenticação usando o snap-in de gerenciamento do AD FS

1.  Abra o snap-in de gerenciamento do AD FS (no Gerenciador de servidores **ferramentas** menu).

2.  Clique em **políticas de autenticação**.

3.  Sob **a autenticação multifator**, clique no **editar** link à direita de **configurações globais**.

4.  Sob **Selecionar métodos de autenticação adicionais**, marque a caixa para AdminName do seu provedor. Clique em **Aplicar**.

5.  Para fornecer um "gatilho" para invocar a autenticação Multifator usando o seu adaptador, em locais de marque ambos **Extranet** e **Intranet**, por exemplo. Clique em **OK**.

### <a name="authenticate-with-mfa-using-your-adapter"></a>Autenticar com o MFA usando o adaptador

Por fim, execute as etapas abaixo para seu adaptador de teste:

1.  Verifique se o tipo de autenticação primária global do AD FS está configurado como **autenticação de formulários** para Extranet e Intranet (Isso torna mais fácil autenticar como um usuário específico).
    
    1.  No gerenciamento do AD FS snap-in, sob **políticas de autenticação**, no **autenticação primária** área, clique em **editar** lado **configurações globais do**.
        
        1.  Ou simplesmente clique a **primário** guia da política de autenticação multifator da interface do usuário.

2.  Certifique-se **autenticação de formulários** é a única opção marcada para ambos os **Extranet** e o **Intranet** método de autenticação.  Clique em **OK**.

3.  Página de logon html aberto iniciado pelo IDP (https://\<fsname\>/adfs/ls/idpinitiatedsignon.htm) e entre como um usuário válido do AD em seu ambiente de teste.

4.  Insira as credenciais para autenticação primária.

5.  Você deve ver os formulários MFA a página com texto de desafio de exemplo são exibidos.
    
    1.  Se você tiver mais de um adaptador configurado, você verá a página de opção MFA com o nome amigável.

Você deve ver um entrar com êxito ao digitar "adfabric" página de autenticação do MFA.

![entrar com adaptador](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "entrar com adaptador")

![entrar com adaptador](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "entrar com adaptador")

## <a name="see-also"></a>Consulte também

#### <a name="other-resources"></a>Outros recursos

[Métodos de autenticação adicionais](https://msdn.microsoft.com/en-us/library/dn758113\(v=msdn.10\))  
[Gerencie riscos com autenticação multifator adicional para aplicativos confidenciais](https://msdn.microsoft.com/en-us/library/dn280949\(v=msdn.10\))

