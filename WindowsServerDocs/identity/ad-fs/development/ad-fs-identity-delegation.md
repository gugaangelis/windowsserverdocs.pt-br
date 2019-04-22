---
title: Cenário de delegação de identidade com o AD FS
description: Este cenário descreve um aplicativo que precisa acessar recursos de back-end que necessita da cadeia de delegação de identidade para executar verificações de controle de acesso.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/22/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: b82d5fd749ac874d09bc54123727aaf902c4d778
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819847"
---
# <a name="identity-delegation-scenario-with-ad-fs"></a>Cenário de delegação de identidade com o AD FS


[Começando com o .NET Framework 4.5, Windows Identity Foundation (WIF) foi totalmente integrado ao .NET Framework. A versão do WIF abordado por este tópico, o WIF 3.5 foi preterida e só deve ser usada durante o desenvolvimento no .NET Framework 3.5 SP1 ou o .NET Framework 4. Para obter mais informações sobre o WIF no .NET Framework 4.5, também conhecida como WIF 4.5, consulte a documentação do Windows Identity Foundation no guia de desenvolvimento do .NET Framework 4.5.] 

Este cenário descreve um aplicativo que precisa acessar recursos de back-end que necessita da cadeia de delegação de identidade para executar verificações de controle de acesso. Uma cadeia de delegação de identidade simples consiste em obter as informações sobre o chamador inicial e a identidade do chamador imediato.

Com o modelo de delegação de Kerberos na plataforma Windows hoje, os recursos de back-end têm acesso somente para a identidade do chamador imediato e não para que um chamador inicial. Esse modelo é conhecido como o modelo de subsistema confiável. O WIF mantém a identidade do chamador inicial, bem como o chamador imediato da cadeia de delegação usando a propriedade de ator.

O diagrama a seguir ilustra um cenário de delegação de identidade típico em que um funcionário da Fabrikam acessa recursos expostos em um aplicativo de Contoso.com.

![Identidade](media/ad-fs-identity-delegation/id1.png)

Os usuários fictícios participação neste cenário são:

- Frank: Um funcionário da Fabrikam que deseja acessar recursos da Contoso.
- Daniel: Um desenvolvedor de aplicativo da Contoso que implementa as alterações necessárias no aplicativo.
- ADAM: O administrador de TI da Contoso.

Os componentes envolvidos neste cenário são:

- web1: Um aplicativo Web com links para recursos de back-end que exigem a identidade delegada do chamador inicial. Esse aplicativo é compilado com o ASP.NET.
- Um serviço Web que acessa um SQL Server, que exige que a identidade delegada do chamador inicial, juntamente com as do chamador imediato. Esse serviço é criado com o WCF.
- sts1: Um STS que está na função de provedor de declarações e emite declarações que são esperadas pelo aplicativo (web1). Ele estabeleceu a relação de confiança com Fabrikam.com e também com o aplicativo.
- sts2: Um STS que está na função de provedor de identidade para Fabrikam.com e fornece um ponto de extremidade que o funcionário a Fabrikam usa para autenticar. Ele estabeleceu a relação de confiança com Contoso.com para que os funcionários da Fabrikam têm permissão para acessar recursos na Contoso.com.

>[!NOTE] 
>O termo "Token ActAs", que é geralmente usado neste cenário, se refere a um token que é emitido por um STS e contém a identidade do usuário. A propriedade de ator contém a identidade do STS.

Conforme mostrado no diagrama anterior, o fluxo nesse cenário é:


1. O aplicativo da Contoso é configurado para obter um token ActAs que contém a identidade do funcionário Fabrikam e a identidade do chamador imediato na propriedade de ator. Daniel implementou essas mudanças no aplicativo.
2. O aplicativo da Contoso é configurado para passar o token ActAs para o serviço de back-end. Daniel implementou essas mudanças no aplicativo.
3. O serviço Web da Contoso é configurado para validar o token ActAs chamando sts1. ADAM habilitou sts1 processar solicitações de delegação.
4. Usuário da Fabrikam Frank acessa o aplicativo da Contoso e recebe acesso a recursos de back-end.

## <a name="set-up-the-identity-provider-ip"></a>Configurar o provedor de identidade (IP)

Há três opções disponíveis para o administrador de Fabrikam.com, Frank:


1. Comprar e instalar um produto do STS, como serviços de Federação do Active Directory® (AD FS).
2. Assine um produto do STS de nuvem como o STS do LiveID.
3. Crie um STS personalizado usando o WIF.

Para este cenário de exemplo, vamos supor que Frank seleciona a opção 1 e instala o AD FS como o IP-STS. Ele também configura um ponto de extremidade, chamado \windowsauth, para autenticar os usuários. Referindo-se a documentação do produto do AD FS e se comunicando com o Adam, o administrador de TI da Contoso, Frank estabelece a relação de confiança com o domínio Contoso.com.

## <a name="set-up-the-claims-provider"></a>Configurar o provedor de declarações

As opções disponíveis para o administrador de Contoso.com, Adam, são as mesmas descritas anteriormente para o provedor de identidade. Para este cenário de exemplo, vamos supor que o Adam seleciona a opção 1 e instala o AD FS 2.0 como o RP-STS.

## <a name="set-up-trust-with-the-ip-and-application"></a>Configurar a relação de confiança com o IP e o aplicativo

Consultando a documentação do AD FS, Adam estabelece a relação de confiança entre o aplicativo e Fabrikam.com.

## <a name="set-up-delegation"></a>Configurar a delegação

O AD FS fornece processamento de delegação. Consultando a documentação do AD FS, Adam permite o processamento de tokens de ActAs.

## <a name="application-specific-changes"></a>Alterações específicas do aplicativo

As seguintes alterações devem ser feitas para adicionar suporte para delegação de identidade para um aplicativo existente. Daniel usa o WIF para fazer essas alterações.


- Armazenar em cache o token de bootstrap que web1 recebido do sts1.
- Use CreateChannelActingAs com o token emitido para criar um canal para o serviço Web de back-end.
- Chame o método do serviço de back-end.

## <a name="cache-the-bootstrap-token"></a>Armazenar em cache o Token de Bootstrap

O token de inicialização é o token inicial emitido pelo STS e o aplicativo extrai declarações dele. Nesse cenário de exemplo, esse token é emitido por sts1 para o usuário de Francisco e o aplicativo armazena em cache. O exemplo de código a seguir mostra como recuperar um bootstrap token em um aplicativo ASP.NET:

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
O WIF fornece um método [CreateChannelActingAs](https://msdn.microsoft.com/library/ee733863.aspx), que cria um canal do tipo especificado que aumenta as solicitações de emissão de token com o token de segurança especificado como um elemento ActAs. Você pode passar o token de bootstrap para esse método e, em seguida, chame o método de serviço necessárias no canal retornado. Nesse cenário de exemplo, a identidade de Frank tem o [ator](https://msdn.microsoft.com/library/microsoft.identitymodel.claims.iclaimsidentity.actor.aspx) propriedade definida como a identidade do web1.

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
## <a name="web-service-specific-changes"></a>Alterações de específicos do serviço da Web

Uma vez que o serviço Web é criado com o WCF e habilitado para WIF, depois que a associação está configurada com IssuedSecurityTokenParameters com o endereço do emissor adequado, a validação do ActAs é manipulada automaticamente pelo WIF. 

O serviço Web expõe os métodos específicos necessitados para o aplicativo. Não há nenhuma alteração de código específico necessária no serviço. O exemplo de código a seguir mostra a configuração do serviço Web com IssuedSecurityTokenParameters:

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
[Desenvolvimento do AD FS](../../ad-fs/AD-FS-Development.md)  
