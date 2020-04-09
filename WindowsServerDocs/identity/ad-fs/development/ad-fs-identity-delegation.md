---
title: Cenário de delegação de identidade com AD FS
description: Este cenário descreve um aplicativo que precisa acessar recursos de back-end que exigem a cadeia de delegação de identidade para executar verificações de controle de acesso.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: c36de1c3565be7f0f0e6c6203a21345f3d227e96
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857319"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Cenário de delegação de identidade com AD FS


[Começando com o .NET Framework 4,5, o Windows Identity Foundation (WIF) foi totalmente integrado ao .NET Framework. A versão do WIF abordada por este tópico, WIF 3,5, foi preterida e só deve ser usada durante o desenvolvimento no .NET Framework 3,5 SP1 ou no .NET Framework 4. Para obter mais informações sobre o WIF no .NET Framework 4,5, também conhecido como WIF 4,5, consulte a documentação do Windows Identity Foundation no guia de desenvolvimento do .NET Framework 4,5.] 

Este cenário descreve um aplicativo que precisa acessar recursos de back-end que exigem a cadeia de delegação de identidade para executar verificações de controle de acesso. Uma cadeia de delegação de identidade simples geralmente consiste nas informações no chamador inicial e na identidade do chamador imediato.

Com o modelo de delegação do Kerberos na plataforma Windows hoje, os recursos de back-end têm acesso apenas à identidade do chamador imediato, e não ao chamador inicial. Esse modelo é conhecido como o modelo de subsistema confiável. O WIF mantém a identidade do chamador inicial, bem como o chamador imediato na cadeia de delegação usando a propriedade actor.

O diagrama a seguir ilustra um cenário de delegação de identidade típico no qual um funcionário da Fabrikam acessa os recursos expostos em um aplicativo Contoso.com.

![Identity](media/ad-fs-identity-delegation/id1.png)

Os usuários fictícios que participam desse cenário são:

- Frank: um funcionário da Fabrikam que deseja acessar os recursos da contoso.
- Daniel: um desenvolvedor de aplicativos da Contoso que implementa as alterações necessárias no aplicativo.
- Adam: o administrador de ti da contoso.

Os componentes envolvidos neste cenário são:

- web1: um aplicativo Web com links para recursos de back-end que exigem a identidade delegada do chamador inicial. Esse aplicativo é criado com ASP.NET.
- Um serviço Web que acessa um SQL Server, que requer a identidade delegada do chamador inicial, junto com o chamador imediato. Esse serviço é criado com o WCF.
- STS1: um STS que está na função do provedor de declarações e emite declarações que são esperadas pelo aplicativo (web1). Ele estabeleceu confiança com Fabrikam.com e também com o aplicativo.
- sts2: um STS que está na função de provedor de identidade para Fabrikam.com e fornece um ponto de extremidade que o funcionário da Fabrikam usa para autenticar. Ele estabeleceu confiança com Contoso.com para que os funcionários da Fabrikam tenham permissão para acessar recursos no Contoso.com.

>[!NOTE] 
>O termo "token ActAs", que é usado com frequência neste cenário, refere-se a um token emitido por um STS e que contém a identidade do usuário. A propriedade actor contém a identidade do STS.

Conforme mostrado no diagrama anterior, o fluxo nesse cenário é:


1. O aplicativo contoso é configurado para obter um token ActAs que contém a identidade do funcionário da Fabrikam e a identidade do chamador imediato na propriedade ator. Daniel implementou essas alterações no aplicativo.
2. O aplicativo contoso é configurado para passar o token ActAs para o serviço de back-end. Daniel implementou essas alterações no aplicativo.
3. O serviço Web da Contoso está configurado para validar o token ActAs chamando STS1. Adam habilitou STS1 para processar solicitações de delegação.
4. O usuário da Fabrikam Frank acessa o aplicativo contoso e recebe acesso aos recursos de back-end.

## <a name="set-up-the-identity-provider-ip"></a>Configurar o provedor de identidade (IP)

Há três opções disponíveis para o administrador do Fabrikam.com, Frank:


1. Adquira e instale um produto STS, como Active Directory&reg; serviços de Federação (AD FS).
2. Assine um produto STS de nuvem, como o LiveID STS.
3. Crie um STS personalizado usando o WIF.

Para este cenário de exemplo, vamos supor que Frank selecione opção 1 e instale AD FS como o IP-STS. Ele também configura um ponto de extremidade, chamado \windowsauth, para autenticar os usuários. Consultando a documentação do produto AD FS e conversando com o Adam, o administrador de ti da Contoso, Frank estabelece confiança com o domínio Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurar o provedor de declarações

As opções disponíveis para o administrador do Contoso.com, Adam, são as mesmas descritas anteriormente para o provedor de identidade. Para este cenário de exemplo, vamos supor que o Adam selecione a opção 1 e instale AD FS 2,0 como RP-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Configurar a confiança com o IP e o aplicativo

Fazendo referência à documentação do AD FS, a Adam estabelece a confiança entre o Fabrikam.com e o aplicativo.

## <a name="set-up-delegation"></a>Configurar delegação

AD FS fornece o processamento de delegação. Fazendo referência à documentação do AD FS, o Adam permite o processamento de tokens ActAs.

## <a name="application-specific-changes"></a>Alterações específicas do aplicativo

As alterações a seguir devem ser feitas para adicionar suporte para delegação de identidade a um aplicativo existente. Daniel usa WIF para fazer essas alterações.


- Armazenar em cache o token de inicialização que web1 recebido de STS1.
- Use CreateChannelActingAs com o token emitido para criar um canal para o serviço Web de back-end.
- Chame o método do serviço de back-end.

## <a name="cache-the-bootstrap-token"></a>Armazenar em cache o token de inicialização

O token de Bootstrap é o token inicial emitido pelo STS e o aplicativo extrai declarações dele. Neste cenário de exemplo, esse token é emitido por STS1 para o usuário Frank e o aplicativo o armazena em cache. O exemplo de código a seguir mostra como recuperar um token de Bootstrap em um aplicativo ASP.NET:

```
// Get the Bootstrap Token
SecurityToken bootstrapToken = null;

IClaimsPrincipal claimsPrincipal = Thread.CurrentPrincipal as IClaimsPrincipal;
if ( claimsPrincipal != null )
{
    IClaimsIdentity claimsIdentity = (IClaimsIdentity)claimsPrincipal.Identity;
    bootstrapToken = claimsIdentity.BootstrapToken;
}
```
O WIF fornece um método, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), que cria um canal do tipo especificado que aumenta as solicitações de emissão de token com o token de segurança especificado como um elemento actas. Você pode passar o token Bootstrap para esse método e, em seguida, chamar o método de serviço necessário no canal retornado. Neste cenário de exemplo, a identidade de Frank tem a propriedade [actor](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) definida como web1's Identity.

O trecho de código a seguir mostra como chamar o serviço Web com [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) e, em seguida, chamar um dos métodos do serviço, ComputeResponse, no canal retornado:

```
// Get the channel factory to the backend service from the application state
ChannelFactory<IService2Channel> factory = (ChannelFactory<IService2Channel>)Application[Global.CachedChannelFactory];

// Create and setup channel to talk to the backend service
IService2Channel channel;
lock (factory)
{
// Setup the ActAs to point to the caller's token so that we perform a 
// delegated call to the backend service
// on behalf of the original caller.
    channel = factory.CreateChannelActingAs<IService2Channel>(callerToken);
}

string retval = null;

// Call the backend service and handle the possible exceptions
try
{
    retval = channel.ComputeResponse(value);
    channel.Close();
} catch (Exception exception)
{
    StringBuilder sb = new StringBuilder();
    sb.AppendLine("An unexpected exception occurred.");
    sb.AppendLine(exception.StackTrace);
    channel.Abort();
    retval = sb.ToString();
}

```
## <a name="web-service-specific-changes"></a>Alterações específicas do serviço Web

Como o serviço Web é criado com o WCF e habilitado para o WIF, depois que a associação é configurada com IssuedSecurityTokenParameters com o endereço de emissor apropriado, a validação do ActAs é manipulada automaticamente pelo WIF. 

O serviço Web expõe os métodos específicos necessários para o aplicativo. Não há nenhuma alteração de código específica necessária no serviço. O exemplo de código a seguir mostra a configuração do serviço Web com IssuedSecurityTokenParameters:

```
// Configure the issued token parameters with the correct settings
IssuedSecurityTokenParameters itp = new IssuedSecurityTokenParameters( "http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1" );
itp.IssuerMetadataAddress = new EndpointAddress( "http://localhost:6000/STS/mex" );
itp.IssuerAddress = new EndpointAddress( "http://localhost:6000/STS" );

// Create the security binding element
SecurityBindingElement sbe = SecurityBindingElement.CreateIssuedTokenForCertificateBindingElement( itp );
sbe.MessageSecurityVersion = MessageSecurityVersion.WSSecurity11WSTrust13WSSecureConversation13WSSecurityPolicy12BasicSecurityProfile10;

// Create the HTTP transport binding element
HttpTransportBindingElement httpBE = new HttpTransportBindingElement();

// Create the custom binding using the prepared binding elements
CustomBinding binding = new CustomBinding( sbe, httpBE );

using ( ServiceHost host = new ServiceHost( typeof( Service2 ), new Uri( "http://localhost:6002/Service2" ) ) )
{
    host.AddServiceEndpoint( typeof( IService2 ), binding, "" );
    host.Credentials.ServiceCertificate.SetCertificate( "CN=localhost", StoreLocation.LocalMachine, StoreName.My );

// Enable metadata generation via HTTP GET
    ServiceMetadataBehavior smb = new ServiceMetadataBehavior();
    smb.HttpGetEnabled = true;
    host.Description.Behaviors.Add( smb );
    host.AddServiceEndpoint( typeof( IMetadataExchange ), MetadataExchangeBindings.CreateMexHttpBinding(), "mex" );

// Configure the service host to use WIF
    ServiceConfiguration configuration = new ServiceConfiguration();
    configuration.IssuerNameRegistry = new TrustedIssuerNameRegistry();

    FederatedServiceCredentials.ConfigureServiceHost( host, configuration );

    host.Open();

    Console.WriteLine( "Service2 started, press ENTER to stop ..." );
    Console.ReadLine();

    host.Close();
}
```

## <a name="next-steps"></a>{1&gt;{2&gt;Próximas etapas&lt;2}&lt;1}
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  
