---
title: "Cenário de delegação de identidade com o AD FS"
description: "Este cenário descreve um aplicativo que precisa acessar recursos de back-end que exigem a cadeia de delegação de identidade para executar verificações de controle de acesso."
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: c16a2bf1b8a48ff267e71ff29f18b5e5cda003e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/28/2018
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Cenário de delegação de identidade com o AD FS


[Começando com o .NET Framework 4.5, Windows Identity Foundation WIF () foi totalmente integrado ao .NET Framework. A versão do WIF abordado neste tópico, 3.5 WIF, foi preterida e só deve ser usada ao desenvolver contra o .NET Framework 3.5 SP1 ou o .NET Framework 4. Para saber mais sobre WIF no .NET Framework 4.5, também conhecido como WIF 4.5, consulte a documentação do Windows Identity Foundation no guia de desenvolvimento do .NET Framework 4.5.] 

Este cenário descreve um aplicativo que precisa acessar recursos de back-end que exigem a cadeia de delegação de identidade para executar verificações de controle de acesso. Uma cadeia de delegação de identidade simples consiste nas informações sobre o chamador inicial e a identidade do chamador imediato.

Com o modelo de delegação Kerberos na plataforma do Windows hoje, os recursos de back-end têm acesso somente para a identidade do chamador imediato e não para que o chamador inicial. Esse modelo é conhecido como o modelo do subsistema confiável. WIF mantém a identidade do chamador inicial, bem como o chamador imediato na cadeia de delegação usando a propriedade ator.

O diagrama a seguir ilustra um cenário de delegação de identidade típico em que um funcionário da Fabrikam acessa recursos expostos em um aplicativo Contoso.com.

![Identidade](media/ad-fs-identity-delegation/id1.png)

Os usuários fictícios participação neste cenário são:

- Frank: Um funcionário da Fabrikam que deseja acessar recursos da Contoso.
- Daniel: Contoso desenvolvedor de aplicativo de que implementa as alterações necessárias no aplicativo.
- ADAM: O Contoso administrador de TI.

Os componentes envolvidos neste cenário são:

- Web1: um aplicativo Web com links para recursos de back-end que exigem a identidade delegada do chamador inicial. Este aplicativo é compilado com ASP.NET.
- Um serviço Web que acessa um servidor SQL, que exige que a identidade delegada do chamador inicial, juntamente com que o chamador imediato. Esse serviço é criado com o WCF.
- sts1: um STS que está na função do provedor de declarações e emite requerimentos judiciais ou Extrajudiciais esperados pelo aplicativo (web1). Ele estabelece confiança com Fabrikam.com e também com o aplicativo.
- sts2: um STS que está na função do provedor de identidade para Fabrikam.com e fornece um ponto de extremidade que o funcionário da Fabrikam usa para autenticar. Ele estabelece confiança com Contoso.com para que os funcionários da Fabrikam têm permissão para acessar recursos em Contoso.com.

>[!NOTE] 
>O termo "Token ActAs", que é usado com frequência neste cenário, se refere a um token que é emitido por um STS e contém a identidade do usuário. A propriedade ator contém a identidade do STS.

Conforme mostrado no diagrama anterior, o fluxo neste cenário é:


1. O aplicativo da Contoso é configurado para obter um token de ActAs que contém a identidade do funcionário da Fabrikam e identidade do chamador imediato na propriedade ator. Daniel implementou essas alterações para o aplicativo.
2. O aplicativo Contoso é configurado para passar o token ActAs para o serviço de back-end. Daniel implementou essas alterações para o aplicativo.
3. O serviço Web de Contoso está configurado para validar o token ActAs chamando sts1. ADAM permitiu sts1 processar solicitações de delegação.
4. Usuário Fabrikam Frank acessa o aplicativo Contoso e recebe acesso a recursos de back-end.

## <a name="set-up-the-identity-provider-ip"></a>Configurar o provedor de identidade (IP)

Há três opções disponíveis para o administrador Fabrikam.com, Frank:


1. Comprar e instalar um produto STS como serviços de Federação do Active Directory® (AD FS).
2. Assine um produto de STS de nuvem como LiveID STS.
3. Crie um STS personalizado usando WIF.

Para esse cenário de exemplo, presumimos que Frank seleciona option1 e instala o AD FS como o IP-STS. Ele também define um ponto final, denominado \windowsauth, para autenticar os usuários. Fazendo referência a documentação do produto do AD FS e falar com Adam, o administrador de TI da Contoso, Frank estabelece confiança com o domínio Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurar o provedor de declarações

As opções disponíveis para o administrador Contoso.com, Adam, são o mesmo, conforme descrito anteriormente para o provedor de identidade. Para esse cenário de exemplo, presumimos que Adam seleciona a opção 1 e instala o AD FS 2.0 como o RP-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Configurar a relação de confiança com o aplicativo e IP

Fazendo referência a documentação do AD FS, Adam estabelece confiança entre Fabrikam.com e o aplicativo.

## <a name="set-up-delegation"></a>Configurar delegação

AD FS fornece o processamento de delegação. Fazendo referência a documentação do AD FS, Adam permite que o processamento de ActAs tokens.

## <a name="application-specific-changes"></a>Mudanças específicas do aplicativo

As seguintes alterações devem ser feitas para adicionar suporte a delegação de identidade para um aplicativo existente. Daniel usa WIF para fazer essas alterações.


- Armazenar em cache o token de inicialização que web1 recebida do sts1.
- Use CreateChannelActingAs com o token emitido para criar um canal para o serviço Web de back-end.
- Chame o método do serviço de back-end.

## <a name="cache-the-bootstrap-token"></a>Armazenar em cache o Token de inicialização

O token de inicialização é o token inicial emitido pelo STS, e o aplicativo extrai requerimentos judiciais ou Extrajudiciais dele. Neste cenário de exemplo, esse token emitido pelo sts1 para o usuário Frank e o aplicativo armazena em cache. O exemplo de código a seguir mostra como recuperar um bootstrap token em um aplicativo de ASP.NET:

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
WIF fornece um método, [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), que cria um canal do tipo especificado que amplia a solicitações de emissão de token com o token de segurança especificado como um elemento ActAs. Você pode passar o token de inicialização para esse método e, em seguida, chame o método de serviço necessárias no canal retornado. Neste cenário de exemplo, a identidade de Frank tem o [ator](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) propriedade definida a identidade do web1.

O trecho de código a seguir mostra como chamar o serviço Web com [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx) e, em seguida, chame um dos métodos do serviço, ComputeResponse, no canal retornado:

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
## <a name="web-service-specific-changes"></a>Mudanças de específicas do serviço Web

Desde que o serviço Web é criado com o WCF e habilitado para WIF, depois que a associação é configurada com IssuedSecurityTokenParameters com o endereço do emissor adequado, a validação do ActAs é manipulada automaticamente pelo WIF. 

O serviço Web expõe os métodos específicos necessitados ao aplicativo. Não há nenhuma alteração de código específico necessária no serviço. O exemplo de código a seguir mostra a configuração do serviço da Web com IssuedSecurityTokenParameters:

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

## <a name="next-steps"></a>Próximas etapas
[AD FS desenvolvimento](../../ad-fs/AD-FS-Development.md)  
