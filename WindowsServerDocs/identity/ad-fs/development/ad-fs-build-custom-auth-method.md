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
ms.openlocfilehash: fc71ca2b8d130ab00014f850ccae25e9138d501b
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867565"
---
# <a name="build-a-custom-authentication-method-for-ad-fs-in-windows-server"></a>Criar um método de autenticação personalizado para AD FS no Windows Server

Este tutorial fornece instruções para implementar um método de autenticação personalizado para AD FS no Windows Server 2012 R2. Para obter mais informações, consulte [métodos de autenticação adicionais](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\)).


> [!WARNING]
> O exemplo que você pode criar aqui é&nbsp;apenas para fins educacionais. &nbsp;Essas instruções são para a implementação mais simples e mínima possível para expor os elementos necessários do modelo.&nbsp; Não há nenhum back-end de autenticação, processamento de erro ou dados de configuração. 
> <P></P>



## <a name="setting-up-the-development-box"></a>Configurando a caixa de desenvolvimento

O passo a passo usa o Visual Studio 2012.  O projeto pode ser criado usando qualquer ambiente de desenvolvimento que possa criar uma classe .NET para Windows. O projeto deve ter como destino o .NET 4,5 porque os métodos **BeginAuthentication** e **TryEndAuthentication** usam o tipo **System. Security. Claims. Claim**, parte da .NET Framework versão 4.5. há uma referência necessária para o projeto:


<table>
<colgroup>
<col style="width: 33%" />
<col style="width: 33%" />
<col style="width: 33%" />
</colgroup>
<tbody>
<tr class="odd">
<td><p><strong>DLL de referência</strong></p></td>
<td><p><strong>Onde encontrá-lo</strong></p></td>
<td><p><strong>Necessário para</strong></p></td>
</tr>
<tr class="even">
<td><p>Microsoft. IdentityServer. Web. dll</p></td>
<td><p>A dll está localizada em%windir%\ADFS em um servidor Windows Server 2012 R2 no qual o AD FS foi instalado.</p>
<p></p>
<p>Essa DLL deve ser copiada para o computador de desenvolvimento e uma referência explícita criada no projeto.</p></td>
<td><p>Tipos de interface, incluindo IAuthenticationContext, IProofData</p></td>
</tr>
</tbody>
</table>


## <a name="create-the-provider"></a>Criar o provedor

1.  No Visual Studio 2012: Escolha arquivo-\>novo-\>projeto...

2.  Selecione biblioteca de classes e verifique se você está direcionando para o .NET 4,5.

    ![criar o provedor](media/ad-fs-build-custom-auth-method/Dn783423.71a57ae1-d53d-462b-a846-5b3c02c7d3f2(MSDN.10).jpg "criar o provedor")

3.  Faça uma cópia de **Microsoft. IdentityServer. Web. dll** de% windir%\\ADFS no servidor Windows Server 2012 R2 em que o AD FS foi instalado e cole-o na pasta do projeto no computador de desenvolvimento.

4.  Em **Gerenciador de soluções**, clique com o botão direito do mouse em **referências** e **adicione referência...**

5.  Navegue até sua cópia local de **Microsoft. IdentityServer. Web. dll** e **adicione...**

6.  Clique em **OK** para confirmar a nova referência:

    ![criar o provedor](media/ad-fs-build-custom-auth-method/Dn783423.f18df353-9259-4744-b4b6-dd780ce90951(MSDN.10).jpg "criar o provedor")

    Agora você deve estar configurado para resolver todos os tipos necessários para o provedor. 

7.  Adicione uma nova classe ao seu projeto (clique com o botão direito do mouse no projeto, **adicione... Classe...** ) e dê a ele um nome como **myadapter**, mostrado abaixo:

    ![criar o provedor](media/ad-fs-build-custom-auth-method/Dn783423.6b6a7a8b-9d66-40c7-8a86-a2e3b9e14d09(MSDN.10).jpg "criar o provedor")

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

    Agora você deve ser capaz de F12 (clique com o botão direito do mouse em definição) em IAuthenticationAdapter para ver o conjunto de membros de interface necessários. 

    Em seguida, você pode fazer uma implementação simples desses.

9.  Substitua todo o conteúdo de sua classe pelo seguinte:

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

10. Ainda não estamos prontos para compilar... Há mais duas interfaces a serem acessadas.

    Adicione mais duas classes ao seu projeto: uma é para os metadados e a outra para o formulário de apresentação.  Você pode adicioná-los no mesmo arquivo que a classe acima.

        class MyMetadata : IAuthenticationAdapterMetadata
         {

         }

         class MyPresentationForm : IAdapterPresentationForm
         {

         }

11. Em seguida, você pode adicionar os membros necessários para cada um. Primeiro, os metadados (com comentários embutidos úteis)

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

         /// Returns an array indicating the type of claim that the adapter uses to identify the user being authenticated.
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

    Em seguida, o formulário de apresentação:

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


~~~
     }
~~~

12. Observe o ' todo ' para o elemento **Resources. FormPageHtml** acima. 

   Você pode corrigi-lo em um minuto, mas primeiro vamos adicionar as instruções de retorno finais necessárias, com base nos tipos implementados recentemente, à sua classe inicial myadapter.  Para fazer isso, adicione os itens em *itálico* abaixo à implementação de IAuthenticationAdapter existente:

       classe myadapter: IAuthenticationAdapter {metadados de IAuthenticationAdapterMetadata públicos {//get {retornar <instance of IAuthenticationAdapterMetadata derived class>novo;}     Get {retornar novo MyMetadata ();}     }

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

13. Agora, para o arquivo de recurso que contém o fragmento HTML. Crie um novo arquivo de texto na pasta do projeto com o seguinte conteúdo:

       <div id="loginArea">
        <form method="post" id="loginForm" >
        <!-- These inputs are required by the presentation framework. Do not modify or remove -->
        <input id="authMethod" type="hidden" name="AuthMethod" value="%AuthMethod%"/>
        <input id="context" type="hidden" name="Context" value="%Context%"/>
        <!-- End inputs are required by the presentation framework. -->
        <p id="pageIntroductionText">Esse conteúdo é fornecido pelo adaptador de exemplo do MFA. As entradas de desafio devem ser apresentadas abaixo.</p>
        <label for="challengeQuestionInput" class="block">Texto da pergunta</label>
        <input id="challengeQuestionInput" name="ChallengeQuestionAnswer" type="text" value="" class="text" placeholder="Answer placeholder" />
        <div id="submissionArea" class="submitMargin">
        <input id="submitButton" type="submit" name="Submit" value="Submit" onclick="return AuthPage.submitAnswer()"/>
        </div>
        </form>
        <div id="intro" class="groupMargin">
        <p id="supportEmail">Informações de suporte</p>
        </div>
        <script type="text/javascript" language="JavaScript">
        //<![CDATA[
        function AuthPage() { }
        AuthPage.submitAnswer = function () { return true; };
        //]]>
        </script></div>

14. Em seguida, **selecione projeto\>-adicionar componente... Arquivo** de recursos e nomeie os **recursos**de arquivo e clique em **Adicionar:**

   ![criar o provedor](media/ad-fs-build-custom-auth-method/Dn783423.3369ad8f-f65f-4f36-a6d5-6a3edbc1911a(MSDN.10).jpg "criar o provedor")

15. Em seguida, no arquivo **Resources. resx** , escolha **Adicionar recurso... Adicionar arquivo existente**.  Navegue até o arquivo de texto (que contém o fragmento HTML) que você salvou acima.

   Verifique se o código GetFormHtml resolve o nome do novo recurso corretamente pelo prefixo de nome do arquivo de recursos (arquivo. resx) seguido pelo nome do próprio recurso:

       Cadeia de caracteres pública GetFormHtml (int LCID) {String htmltemplate = Resources. MfaFormHtml;//resxFilename.resourceName retorna htmltemplate;    }

   Agora você deve ser capaz de criar.

## <a name="build-the-adapter"></a>Criar o adaptador

O adaptador deve ser incorporado em um assembly .NET fortemente nomeado que pode ser instalado no GAC no Windows.  Para conseguir isso em um projeto do Visual Studio, conclua as seguintes etapas:

1.  Clique com o botão direito do mouse no nome do projeto em Gerenciador de Soluções e clique em **Propriedades**.

2.  Na guia **assinatura** , marque **assinar o assembly** e escolha **\<novo... em\>** **escolher um arquivo de chave de nome forte:**  Insira um nome de arquivo de chave e uma senha e clique em **OK**.  Em seguida, certifique-se **de que assinar o assembly** esteja marcado e a opção **somente sinal de atraso** esteja desmarcada.  A página de **assinatura** de propriedades deve ter esta aparência:

    ![criar o provedor](media/ad-fs-build-custom-auth-method/Dn783423.0b1a1db2-d64e-4bb8-8c01-ef34296a2668(MSDN.10).jpg "criar o provedor")

3.  Em seguida, Compile a solução.

## <a name="deploy-the-adapter-to-your-ad-fs-test-machine"></a>Implantar o adaptador em seu computador de teste de AD FS

Antes que um provedor externo possa ser invocado pelo AD FS, ele deve ser registrado no sistema.  Os provedores de adaptador devem fornecer um instalador que executa as ações de instalação necessárias, incluindo a instalação no GAC, e o instalador deve dar suporte ao registro no AD FS.  Se isso não for feito, o administrador precisará executar as etapas do Windows PowerShell abaixo.  Essas etapas podem ser usadas no laboratório para habilitar o teste e a depuração.

### <a name="prepare-the-test-ad-fs-machine"></a>Preparar o computador de AD FS de teste

Copie os arquivos e adicione-o ao GAC.

1.  Verifique se você tem um computador com Windows Server 2012 R2 ou uma máquina virtual.

2.  Instale o AD FS serviço de função e configure um farm com pelo menos um nó.

    Para obter etapas detalhadas para configurar um servidor de Federação em um ambiente de laboratório, consulte o [Guia de implantação do Windows server 2012 R2 AD FS](https://msdn.microsoft.com/library/dn486820\(v=msdn.10\)).

3.  Copie as ferramentas Gacutil. exe para o servidor.

    O Gacutil. exe pode ser encontrado em **% HomeDrive\\% Program Files (\\x86)\\Microsoft\\SDKs Windows v\\8.0\\a bin NETFX\\ 4,0 Tools** em um computador com Windows 8.  Você precisará do próprio arquivo **Gacutil. exe** , bem como o **1033**, **en-US**e a outra pasta de recursos localizados abaixo do local das **ferramentas NETFX 4,0** .

4.  Copie seus arquivos de provedor (um ou mais arquivos. dll assinados com nomes fortes) para o mesmo local de pasta que o **Gacutil. exe** (o local é apenas para conveniência)

5.  Adicione seus arquivos. dll ao GAC em cada AD FS servidor de Federação no farm:

    Exemplo: usando a ferramenta de linha de comando GACutil. exe para adicionar uma dll ao GAC:`C:\>.\gacutil.exe /if .\<yourdllname>.dll`

    Para exibir a entrada resultante no GAC:`C:\>.\gacutil.exe /l <yourassemblyname>`

6.  

### <a name="register-your-provider-in-ad-fs"></a>Registrar seu provedor no AD FS

Depois que os pré-requisitos acima forem atendidos, abra uma janela de comando do Windows PowerShell no servidor de Federação e insira os comandos a seguir (Observe que se você estiver usando o farm de servidores de Federação que usa o banco de dados interno do Windows, execute estes comandos no servidor de Federação primário do farm):

1.  `Register-AdfsAuthenticationProvider –TypeName YourTypeName –Name “AnyNameYouWish” [–ConfigurationFilePath (optional)]`

    Em que YourTypeName é o nome do tipo forte do .NET: "YourDefaultNamespace. YourIAuthenticationAdapterImplementationClassName, YourAssemblyName, Version = YourAssemblyVersion, Culture = neutral, PublicKeyToken = YourPublicKeyTokenValue, processorArchitecture = MSIL"

    Isso registrará seu provedor externo no AD FS, com o nome fornecido como AnyNameYouWish acima.

2.  Reinicie o serviço de AD FS (usando o snap-in Serviços do Windows, por exemplo).

3.  Execute o seguinte comando: `Get-AdfsAuthenticationProvider`.

    Isso mostra seu provedor como um dos provedores no sistema.

    Exemplo:

        PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”
        PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter”
        PS C:\>net stop adfssrv
        PS C:\>net start adfssrv

    Se você tiver o serviço de registro de dispositivo habilitado em seu ambiente de AD FS, também execute o seguinte:`PS C:\>net start drs`

    Para verificar o provedor registrado, use o seguinte comando:`PS C:\>Get-AdfsAuthenticationProvider`.

    Isso mostra seu provedor como um dos provedores no sistema.

### <a name="create-the-ad-fs-authentication-policy-that-invokes-your-adapter"></a>Criar a política de autenticação AD FS que invoca o adaptador

#### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Criar a política de autenticação usando o snap-in de gerenciamento de AD FS

1.  Abra o snap-in de gerenciamento de AD FS (no menu **ferramentas** do Gerenciador do servidor).

2.  Clique em **políticas de autenticação**.

3.  No painel central, em **autenticação multifator**, clique no link **Editar** à direita das **configurações globais**.

4.  Em **Selecionar métodos de autenticação adicionais** na parte inferior da página, marque a caixa para o adminname do seu provedor. Clique em **Aplicar**.

5.  Para fornecer um "gatilho" para invocar o MFA usando o adaptador, em **locais** , verifique a **extranet** e a **intranet**, por exemplo. Clique em **OK**. (Para configurar gatilhos por terceira parte confiável, consulte "criar a política de autenticação usando o Windows PowerShell" abaixo.)

6.  Verifique os resultados usando os seguintes comandos:

    Primeiro uso `Get-AdfsGlobalAuthenticationPolicy`. Você deve ver o nome do provedor como um dos valores de AdditionalAuthenticationProvider.

    Em seguida `Get-AdfsAdditionalAuthenticationRule`, use. Você deve ver as regras para extranet e intranet configuradas como resultado de sua seleção de política na interface do usuário do administrador.

#### <a name="create-the-authentication-policy-using-windows-powershell"></a>Criar a política de autenticação usando o Windows PowerShell

1.  Primeiro, habilite o provedor na política global:

    `PS C:\>Set-AdfsGlobalAuthenticationPolicy -AdditionalAuthenticationProvider “YourAuthProviderName”`


~~~
> [!NOTE]
> Note that the value provided for the AdditionalAuthenticationProvider parameter corresponds to the value you provided for the “Name” parameter in the Register-AdfsAuthenticationProvider cmdlet above and to the “Name” property from Get-AdfsAuthenticationProvider cmdlet output. 
> <P></P>


Example:`PS C:\>Set-AdfsGlobalAuthenticationPolicy –AdditionalAuthenticationProvider “MyMFAAdapter”`
~~~

2. Em seguida, configure regras globais ou de terceira parte confiável para disparar MFA:

   Exemplo 1: para criar uma regra global para exigir MFA para solicitações externas:`PS C:\>Set-AdfsAdditionalAuthenticationRule –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'`

   Exemplo 2: para criar regras de MFA para exigir MFA para solicitações externas para uma terceira parte confiável específica.  (Observe que os provedores individuais não podem ser conectados a partes confiáveis individuais no AD FS no Windows Server 2012 R2).

       PS C:\>$rp = Get-AdfsRelyingPartyTrust –Name <Relying Party Name>
       PS C:\>Set-AdfsRelyingPartyTrust –TargetRelyingParty $rp –AdditionalAuthenticationRules 'c:[type == "http://schemas.microsoft.com/ws/2012/01/insidecorporatenetwork", value == "false"] => issue(type = "http://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", value = "http://schemas.microsoft.com/claims/multipleauthn" );'

### <a name="authenticate-with-mfa-using-your-adapter"></a>Autenticar com o MFA usando seu adaptador

Por fim, execute as etapas abaixo para testar seu adaptador:

1.  Verifique se o tipo de autenticação primária global AD FS está configurado como autenticação de formulários para extranet e intranet (isso torna sua demonstração mais fácil de autenticar como um usuário específico)

    1.  No snap-in AD FS, em **políticas de autenticação**, na área **autenticação primária** , clique em **Editar** ao lado de **configurações globais**.

        1.  Ou simplesmente clique na guia **primário** da interface do usuário da **diretiva multifator** .

2.  Verifique se a **autenticação de formulários** é a única opção verificada para a extranet e o método de autenticação de intranet.  Clique em **OK**.

3.  Abra a página HTML de logon iniciada pelo IDP (\<https://\>FSName/adfs/ls/idpinitiatedsignon.htm) e entre como um usuário válido do AD em seu ambiente de teste.

4.  Insira as credenciais para a autenticação primária.

5.  Você deve ver a página de formulários do MFA com exemplos de perguntas de desafio. 

    Se você tiver mais de um adaptador configurado, verá a página de escolha do MFA com seu nome amigável acima.

    ![autenticar com adaptador](media/ad-fs-build-custom-auth-method/Dn783423.c98d2712-cbd3-4cb9-ac03-2838b81c4f63(MSDN.10).jpg "autenticar com adaptador")

    ![autenticar com adaptador](media/ad-fs-build-custom-auth-method/Dn783423.fd3aefc0-ef6c-4a8c-a737-4914c78ff2d2(MSDN.10).jpg "autenticar com adaptador")

Agora você tem uma implementação funcional da interface e tem o conhecimento de como o modelo funciona. Você pode Trym como um exemplo extra para definir pontos de interrupção no BeginAuthentication, bem como o TryEndAuthentication.  Observe como o BeginAuthentication é executado quando o usuário entra pela primeira vez no formulário MFA, enquanto o TryEndAuthentication é disparado em cada envio do formulário.

## <a name="update-the-adapter-for-successful-authentication"></a>Atualizar o adaptador para autenticação bem-sucedida

Mas espere – o adaptador de exemplo nunca será autenticado com êxito\!  Isso ocorre porque nada em seu código retorna NULL para TryEndAuthentication.

Ao concluir os procedimentos acima, você criou uma implementação básica de adaptador e a adicionou a um servidor AD FS.  Você pode obter a página de formulários do MFA, mas ainda não pode ser autenticado porque você ainda não colocou a lógica correta em sua implementação do TryEndAuthentication.  Então, vamos adicionar isso.

Lembre-se da implementação do TryEndAuthentication:

    public IAdapterPresentation TryEndAuthentication(IAuthenticationContext authContext, IProofData proofData, HttpListenerRequest request, out Claim[] outgoingClaims)
     {
     //return new instance of IAdapterPresentationForm derived class
     outgoingClaims = new Claim[0];
     return new MyPresentationForm();

     }

Vamos atualizá-lo para que ele nem sempre retorne MyPresentationForm ().  Para isso, você pode criar um método utilitário simples dentro de sua classe:

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

Em seguida, atualize TryEndAuthentication como mostrado abaixo:

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

Agora você precisa atualizar o adaptador na caixa de teste.  Você deve primeiro desfazer a política de AD FS, em seguida, cancelar o registro de AD FS e reiniciar AD FS, em seguida, remover o. dll do GAC e, em seguida, adicionar o New. dll ao GAC e, em seguida, registrá-lo em AD FS, reiniciar AD FS e reconfigurar a política de AD FS.

## <a name="deploy-and-configure-the-updated-adapter-on-your-test-ad-fs-machine"></a>Implantar e configurar o adaptador atualizado em seu computador de AD FS de teste

### <a name="clear-ad-fs-policy"></a>Limpar política de AD FS

Desmarque todas as caixas de seleção relacionadas à MFA na interface do usuário do MFA, mostrada abaixo e clique em OK.

![limpar política](media/ad-fs-build-custom-auth-method/Dn783423.c111b4e7-5b05-413c-8b0f-222a0e91ac1f(MSDN.10).jpg "limpar política")

### <a name="unregister-provider-windows-powershell"></a>Cancelar registro do provedor (Windows PowerShell)

`PS C:\> Unregister-AdfsAuthenticationProvider –Name “YourAuthProviderName”`

Exemplo`PS C:\> Unregister-AdfsAuthenticationProvider –Name “MyMFAAdapter”`

Observe que o valor que você passa para "Name" é o mesmo valor que o "Name" fornecido ao cmdlet Register-AdfsAuthenticationProvider.  Também é a propriedade "Name" que é saída de Get-AdfsAuthenticationProvider.

Observe que, antes de cancelar o registro de um provedor, você deve remover o provedor do AdfsGlobalAuthenticationPolicy (desmarcando as caixas de seleção nas quais você fez o check-in AD FS Management ou usando o Windows PowerShell.)

Observe que o serviço de AD FS deve ser reiniciado após essa operação.

### <a name="remove-assembly-from-gac"></a>Remover assembly do GAC

1.  Primeiro, use o seguinte comando para encontrar o nome forte totalmente qualificado da entrada:`C:\>.\gacutil.exe /l <yourAdapterAssemblyName>`

    Exemplo`C:\>.\gacutil.exe /l mfaadapter`

2.  Em seguida, use o seguinte comando para removê-lo do GAC:`.\gacutil /u “<output from the above command>”`

    Exemplo`C:\>.\gacutil /u “mfaadapter, Version=1.0.0.0, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

### <a name="add-the-updated-assembly-to-gac"></a>Adicionar o assembly atualizado ao GAC

Certifique-se de colar o. dll atualizado primeiro. `C:\>.\gacutil.exe /if .\MFAAdapter.dll`

### <a name="view-assembly-in-the-gac-cmd-line"></a>Exibir assembly no GAC (linha de comando)

`C:\> .\gacutil.exe /l mfaadapter`

### <a name="register-your-provider-in-ad-fs"></a>Registrar seu provedor no AD FS

1.  `PS C:\>$typeName = "MFAadapter.MyAdapter, MFAadapter, Version=1.0.0.1, Culture=neutral, PublicKeyToken=e675eb33c62805a0, processorArchitecture=MSIL”`

2.  `PS C:\>Register-AdfsAuthenticationProvider -TypeName $typeName -Name “MyMFAAdapter1”`

3.  Reinicie o serviço AD FS.

### <a name="create-the-authentication-policy-using-the-ad-fs-management-snap-in"></a>Criar a política de autenticação usando o snap-in de gerenciamento de AD FS

1.  Abra o snap-in de gerenciamento de AD FS (no menu **ferramentas** do Gerenciador do servidor).

2.  Clique em **políticas de autenticação**.

3.  Em **autenticação multifator**, clique no link **Editar** à direita de **configurações globais**.

4.  Em **Selecionar métodos de autenticação adicionais**, marque a caixa para o adminname do seu provedor. Clique em **Aplicar**.

5.  Para fornecer um "gatilho" para invocar o MFA usando o adaptador, em locais, verifique a **extranet** e a **intranet**, por exemplo. Clique em **OK**.

### <a name="authenticate-with-mfa-using-your-adapter"></a>Autenticar com o MFA usando seu adaptador

Por fim, execute as etapas abaixo para testar seu adaptador:

1.  Verifique se o tipo de autenticação primária global AD FS está configurado como **autenticação de formulários** para extranet e intranet (isso torna mais fácil a autenticação como um usuário específico).

    1.  No snap-in de gerenciamento de AD FS, **em políticas de autenticação**, na área **autenticação primária** , clique em **Editar** ao lado de **configurações globais**.

        1.  Ou simplesmente clique na guia **primário** da interface do usuário da diretiva multifator.

2.  Verifique se a **autenticação de formulários** é a única opção verificada para a **extranet** e o método de autenticação de **intranet** .  Clique em **OK**.

3.  Abra a página HTML de logon iniciada pelo IDP (\<https://\>FSName/adfs/ls/idpinitiatedsignon.htm) e entre como um usuário válido do AD em seu ambiente de teste.

4.  Insira as credenciais para autenticação primária.

5.  Você deve ver a página de formulários do MFA com o exemplo de texto de desafio exibido.

    1.  Se você tiver mais de um adaptador configurado, verá a página de escolha do MFA com seu nome amigável.

Você deverá ver uma entrada bem-sucedida ao inserir "adfabric" na página de autenticação do MFA.

![entrar com adaptador](media/ad-fs-build-custom-auth-method/Dn783423.630d8a91-3bfe-4cba-8acf-03eae21530ee(MSDN.10).jpg "entrar com adaptador")

![entrar com adaptador](media/ad-fs-build-custom-auth-method/Dn783423.c340fa73-f70f-4870-b8dd-07900fea4469(MSDN.10).jpg "entrar com adaptador")

## <a name="see-also"></a>Consulte também

#### <a name="other-resources"></a>Outros recursos

[Métodos de autenticação adicionais](https://msdn.microsoft.com/library/dn758113\(v=msdn.10\))  
[Gerenciar riscos com Autenticação Multifator adicional para aplicativos confidenciais](https://msdn.microsoft.com/library/dn280949\(v=msdn.10\))

