---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: Apêndice A - revisar os requisitos do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 5e90df713f08dd387a2438b34839d16efe6e470f
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191688"
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>Apêndice a: Examinando requisitos do AD FS

Para que os parceiros organizacionais em sua implantação de serviços de Federação do Active Directory (AD FS) podem colaborar com êxito, primeiro certifique-se de que sua infraestrutura de rede corporativa está configurada para dar suporte a requisitos do AD FS para contas, nomeie resolução e certificados. O AD FS tem os seguintes tipos de requisitos:  
  
> [!TIP]  
> Você pode encontrar links de recursos do AD FS adicionais na página [Mapa de Conteúdo do AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) , no TechNet Wiki da Microsoft. Essa página é gerenciada por membros da comunidade do AD FS e é monitorada regularmente pela equipe de produto do AD FS.  
  
## <a name="hardware-requirements"></a>Requisitos de hardware  
Aplicam os seguintes requisitos de hardware mínimos e recomendados para o servidor de Federação e computadores de proxy do servidor de Federação.  
  
|Requisito de hardware|Requisito mínimo|Requisito recomendado|  
|------------------------|-----------------------|---------------------------|  
|Velocidade da CPU|Um núcleo, 1 GHz (gigahertz)|Quatro núcleos, 2 GHz|  
|RAM|1 GB|4 GB|  
|Espaço em disco|50 MB|100 MB|  
  
## <a name="software-requirements"></a>Requisitos de software  
O AD FS se baseia na funcionalidade do servidor que está embutida no sistema operacional Windows Server® 2012.  
  
> [!NOTE]  
> Os serviços de função Serviço de Federação e Proxy do Serviço de Federação não podem coexistir no mesmo computador.  
  
## <a name="certificate-requirements"></a>Requisitos de certificado  
Os certificados exercem a função mais crítica na proteção das comunicações entre os servidores de federação, os proxies dos servidores de federação, os aplicativos com reconhecimento de declaração e os clientes Web. Os requisitos dos certificados variam, dependendo de você configurar um computador servidor de federação ou um computador proxy de servidor de federação, conforme descrito nesta seção.  
  
### <a name="federation-server-certificates"></a>Certificados de servidores de federação  
Os servidores de federação exigem os certificados da tabela a seguir.  
  
|Tipo de certificado|Descrição|O que você precisa saber antes da implantação|  
|--------------------|---------------|------------------------------------------|  
|Certificado SSL|Este é um certificado SSL padrão usado para proteger as comunicações entre servidores de federação e clientes.|Este certificado deve estar associados ao site padrão no IIS (Serviços de Informações da Internet) para um servidor de federação ou proxy de servidor de federação.  Para um proxy de servidor de federação, a associação deve ser configurada no IIS antes da execução bem-sucedida do Assistente de Configuração de Proxy de Servidor de Federação.<br /><br />**Recomendação:** como esse certificado deve ser confiável aos clientes do AD FS, utilize um certificado de autenticação de servidor que seja emitido por uma AC (autoridade de certificação) pública (terceirizada), por exemplo, VeriSign. **Dica:** O Nome da entidade desse certificado é usado para representar o nome do Serviço de Federação de cada instância do AD FS que você implantar. Por esse motivo, talvez você queira considerar a escolha de um Nome da entidade em qualquer certificado emitido por uma nova AC que represente melhor o nome da sua empresa ou organização para os parceiros.|  
|Certificado de comunicação de serviço|Esse certificado habilita a segurança da mensagem WCF (Windows Communication Foundation) para proteger as comunicações entre os servidores de federação.|Por padrão, o certificado SSL é usado como o certificado das comunicações de serviço.  Isso pode ser alterado usando o console de Gerenciamento do AD FS.|  
|Certificado de autenticação de tokens|Este é um certificado X509 padrão usado para autenticar com segurança todos os tokens que o servidor de federação emite.|O certificado de autenticação de tokens deve conter uma chave privada e deve fazer o encadeamento para uma raiz confiável no Serviço de Federação. Por padrão, o AD FS cria um certificado autoassinado. No entanto, você poderá alterar isso depois para um certificado emitido pela AC usando o snap-in Gerenciamento do AD FS, dependendo das necessidades da sua organização.|  
|Certificado de descriptografia de token|Este é um certificado SSL padrão usado para descriptografar todos os tokens recebidos que são criptografados por um servidor de federação do parceiro. Ele também é publicado nos metadados de federação.|Por padrão, o AD FS cria um certificado autoassinado. No entanto, você poderá alterar isso depois para um certificado emitido pela AC usando o snap-in Gerenciamento do AD FS, dependendo das necessidades da sua organização.|  
  
> [!CAUTION]  
> Os certificados usados para a autenticação e descriptografia de tokens são críticos à estabilidade do Serviço de Federação. Visto que uma perda ou remoção não planejada de qualquer certificado configurado para esse fim pode interromper o serviço, você deve fazer o backup de todos os certificados configurados para esse fim.  
  
Para obter mais informações sobre os certificados usados pelos servidores de federação, consulte [Requisitos de certificado para servidores de federação](Certificate-Requirements-for-Federation-Servers.md).  
  
### <a name="federation-server-proxy-certificates"></a>Certificados de proxies de servidores de federação  
Os proxies de servidores de federação exigem os certificados da tabela a seguir.  
  
|Tipo de certificado|Descrição|O que você precisa saber antes da implantação|  
|--------------------|---------------|------------------------------------------|  
|Certificado de autenticação de servidor|Este é um certificado SSL padrão usado para proteger as comunicações entre um proxy de servidor de federação e os computadores cliente da Internet.|Este certificado deve estar associado ao site padrão no IIS (Serviços de Informações da Internet) antes que você possa executar com sucesso o Assistente de Configuração de Proxy de Servidor de Federação do AD FS.<br /><br />**Recomendação:** como esse certificado deve ser confiável aos clientes do AD FS, utilize um certificado de autenticação de servidor que seja emitido por uma AC (autoridade de certificação) pública (terceirizada), por exemplo, VeriSign.<br /><br />**Dica:** O Nome da entidade desse certificado é usado para representar o nome do Serviço de Federação de cada instância do AD FS que você implantar. Por esse motivo, talvez você queira considerar a escolha de um Nome da entidade que represente melhor o nome da sua empresa ou organização para os parceiros.|  
  
Para obter mais informações sobre os certificados usados pelos proxies de servidores de certificação, consulte [Requisitos de certificado para proxies de servidores de federação](Certificate-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="browser-requirements"></a>Requisitos de navegador  
Embora qualquer navegador da Web com funcionalidade de JavaScript possa funcionar com um cliente do AD FS, as páginas da Web que são fornecidas por padrão foram testadas apenas no Internet Explorer versões 7.0, 8.0 e 9.0, Mozilla Firefox 3.0 e Safari 3.1 no Windows. O JavaScript e os cookies devem estar habilitados para que a entrada e a saída do serviço baseadas na Web funcionem corretamente.  
  
A equipe de produto do AD FS na Microsoft testado com êxito as configurações de navegador e o sistema operacional na tabela a seguir.  
  
|Navegador|Windows 7|Windows Vista|  
|-----------|-------------|-----------------|  
|Internet Explorer 7.0|X|X|  
|Internet Explorer 8.0|X|X|  
|Internet Explorer 9.0|X|Não testado|  
|FireFox 3.0|X|X|  
|Safari 3.1|X|X|  
  
> [!NOTE]  
> O AD FS dá suporte às versões de 32 e 64 bits dos navegadores mostrados na tabela acima.  
  
### <a name="cookies"></a>Cookies  
O AD FS cria cookies persistentes e baseados na sessão que devem ser armazenados nos computadores cliente para fornecer conexão, saída do serviço, SSO (logon único) e outras funcionalidades. Portanto, o navegador do cliente deve ser configurado para aceitar cookies. Os cookies usados para autenticação são sempre cookies de sessão HTTPS (Secure Hypertext Transfer Protocol) que são criados para o servidor de origem. Se o navegador do cliente não estiver configurado para permitir esses cookies, o AD FS não poderá funcionar corretamente. Os cookies persistentes são usados para preservar a seleção de usuário do provedor de declarações. Você pode desabilitá-los usando uma definição de configuração do arquivo de configuração das páginas de conexão do AD FS.  
  
Por motivos de segurança, é exigido o suporte a TLS/SSL.  
  
## <a name="network-requirements"></a>Requisitos de rede  
Configurar os serviços de rede a seguir apropriadamente é essencial para uma implantação bem-sucedida do AD FS em sua organização.  
  
### <a name="tcpip-network-connectivity"></a>Conectividade de rede TCP/IP  
Para AD FS funcione, a conectividade de rede TCP/IP deve existir entre o cliente; um controlador de domínio; e os computadores que hospedam o serviço de federação, o Proxy do serviço de Federação (quando ele é usado) e o agente Web do AD FS.  
  
### <a name="dns"></a>DNS  
O serviço de rede principal que é essencial para a operação do AD FS, que não sejam os serviços de domínio do Active Directory (AD DS), é o sistema de nome de domínio (DNS). Quando o DNS é implantado, os usuários podem utilizar nomes de computadores amigáveis e fáceis de lembrar para estabelecer conexão com os computadores e outros recursos das redes IP.  
  
 Windows Server 2008 usa o DNS para resolução de nome em vez da resolução de nome NetBIOS do serviço de nome na Internet do Windows (WINS) que foi usada em redes baseadas no Windows NT 4.0. Ainda é possível usar WINS nos aplicativos que o exigem. No entanto, o AD DS e AD FS exigem a resolução de nomes DNS.  
  
O processo de configuração de DNS para o suporte do AD FS varia, dependendo se:  
  
-   Sua organização já tenha uma infraestrutura de DNS existente. Na maioria dos cenários, o DNS já está configurado em toda a sua rede para que os clientes do navegador da Web da rede corporativa tenham acesso à Internet. Como Internet acesso e resolução de nomes são requisitos do AD FS, essa infraestrutura deve para estar em vigor para sua implantação do AD FS.  
  
-   Você queira adicionar um servidor federado à sua rede corporativa. Para autenticar usuários na rede corporativa, os servidores DNS internos da floresta da rede corporativa devem estar configurados para retornar o CNAME (nome canônico) do servidor interno que está executando o Serviço de Federação. Para obter mais informações, consulte [Name Resolution Requirements for Federation Servers](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   Você pretenda adicionar um proxy de servidor federado à sua rede de perímetro. Quando você deseja autenticar contas de usuário que estão localizadas na rede corporativa da sua organização do parceiro de identidade, os servidores DNS internos da floresta da rede corporativa devem ser configurados para retornar o CNAME de proxy do servidor de Federação interna. Para obter informações sobre como configurar o DNS para acomodar a adição de proxies de servidor de federação, consulte [Name Resolution Requirements for Federation Server Proxies](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   Você esteja configurando o DNS para um ambiente de laboratório de teste. Se você planeja usar o AD FS em um ambiente de laboratório de teste em que nenhum servidor DNS de raiz única é autoritativa, é provável que você terá que configurar encaminhadores DNS para que as consultas de nomes entre duas ou mais florestas sejam encaminhadas adequadamente. Para obter informações gerais sobre como configurar um ambiente de laboratório de teste do AD FS, consulte [passo a passo do AD FS e How To Guides](https://go.microsoft.com/fwlink/?LinkId=180357).  
  
## <a name="attribute-store-requirements"></a>Requisitos de repositório de atributos  
O AD FS requer pelo menos um repositório de atributos a ser usado para autenticar usuários e extrair declarações de segurança para esses usuários. Para obter uma lista de repositórios de atributos com suporte pelo AD FS, consulte [A função dos repositórios de atributos](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md) no Guia de Design do AD FS.  
  
> [!NOTE]  
> Por padrão, o AD FS cria automaticamente um repositório de atributos do Active Directory.  
  
Os requisitos de repositório de atributos dependem de sua organização estar agindo como parceiro de conta (hospedando os usuários federados) ou parceiro de recurso (hospedando o aplicativo federado).  
  
### <a name="adds"></a>AD DS  
Para o AD FS operar com êxito, controladores de domínio na organização do parceiro de conta ou a organização do parceiro de recurso devem estar executando o Windows Server 2003 SP1, Windows Server 2003 R2, Windows Server 2008 ou Windows Server 2012.  
  
Quando o AD FS é instalado e configurado em um computador ingressado em domínio, o repositório de contas de usuário do Active Directory desse domínio é disponibilizado como um repositório de atributos selecionável.  
  
> [!IMPORTANT]  
> Como o AD FS requer a instalação de serviços de informações da Internet (IIS), é recomendável que você não instale o software AD FS em um controlador de domínio em um ambiente de produção para fins de segurança. No entanto, essa configuração é aceita pelo Suporte de Atendimento ao Cliente da Microsoft.  
  
#### <a name="schema-requirements"></a>Requisitos de esquema  
O AD FS não requer alterações de esquema ou modificações de nível funcional no AD DS.  
  
#### <a name="functional-level-requirements"></a>Requisitos de nível funcional  
A maioria dos recursos do AD FS não exigem modificações de nível funcional no AD DS para que funcionem bem. No entanto, o nível funcional do domínio do Windows Server 2008 ou superior é exigido para que a autenticação de certificados de clientes funcione bem, caso o certificado esteja associado explicitamente à conta de usuário do AD DS.  
  
#### <a name="service-account-requirements"></a>Requisitos de conta de serviço  
Se estiver criando um farm de servidores de federação, você deverá criar primeiro uma conta de serviço dedicada e baseada em domínio no AD DS que possa ser usada pelo Serviço de Federação. Depois, você deve configurar cada servidor de federação do farm para usar essa conta. Para obter mais informações sobre esse processo, consulte [Configurar manualmente uma conta de serviço para um farm de servidores de federação](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) no Guia de Implantação do AD FS.  
  
### <a name="ldap"></a>LDAP  
Quando você trabalha com outros repositórios de atributos baseados no protocolo LDAP, você deve estabelecer conexão com um servidor LDAP que suporte a autenticação integrada do Windows. A cadeia de conexão LDAP também deve ser escrita no formato de uma URL LDAP, como descrito no RFC 2255.  
  
### <a name="sql-server"></a>SQL Server  
Para o AD FS operar com êxito, computadores que hospedam o repositório de atributos de servidor SQL Structured Query Language () devem estar executando o Microsoft SQL Server 2005 ou SQL Server 2008. Quando trabalha com repositórios de atributos baseados em SQL, você também deve configurar uma cadeia de conexão.  
  
### <a name="custom-attribute-stores"></a>Repositórios de atributos personalizados  
Você pode desenvolver repositórios de atributos personalizados para habilitar cenários avançados. A linguagem da política interna do AD FS pode referenciar repositórios de atributos personalizados para que qualquer um dos seguintes cenários seja aprimorado:  
  
-   Criar declaração para um usuário autenticado localmente  
  
-   Suplementar declarações para um usuário autenticado externamente  
  
-   Autorizar um usuário a obter um token  
  
-   Autorizar um serviço a obter um token sobre o comportamento de um usuário  
  
Quando você trabalha com um repositório de atributos personalizado, você também pode precisar configurar uma cadeia de conexão. Nessa situação, você pode inserir qualquer código personalizado de que goste para habilitar uma conexão com seu repositório de atributos personalizado. Nessa situação, a cadeia de conexão é um conjunto de pares de nome/valor que são interpretados como implementados pelo desenvolvedor do repositório de atributos personalizado.  
  
Para obter mais informações sobre como desenvolver e usar repositórios de atributos personalizados, consulte [visão geral do Store atributos](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="application-requirements"></a>Requisitos de aplicativo  
Os servidores de federação podem se comunicar com aplicativos de federação e protegê-los, como aplicativos com reconhecimento de declaração.  
  
## <a name="authentication-requirements"></a>Requisitos de autenticação  
O AD FS integra-se naturalmente à autenticação do Windows existente, por exemplo, a autenticação Kerberos, NTLM, cartões inteligentes e certificados de cliente X.509 v3. Os servidores de federação usam a autenticação Kerberos padrão para autenticar um usuário em um domínio. Os clientes podem se autenticar usando a autenticação baseada em formulários, autenticação com cartão inteligente e autenticação integrada do Windows, dependendo de como a autenticação está configurada.  
  
A função do proxy do servidor de federação do AD FS possibilita um cenário no qual o usuário se autentica externamente usando a autenticação de cliente SSL. Você também pode configurar a função do servidor de federação para exigir autenticação de cliente SSL, embora tipicamente a mais perfeita experiência de usuário seja obtida pela configuração do servidor de federação de conta para a autenticação integrada do Windows. Nessa situação, o AD FS não tem controle sobre as credenciais que o usuário emprega para o logon na área de trabalho do Windows.  
  
### <a name="smart-card-logon"></a>Logon com cartão inteligente  
Embora o AD FS pode impor o tipo de credenciais que ele usa para autenticação (senhas, autenticação de cliente SSL ou autenticação integrada do Windows), ele não impõe diretamente a autenticação com cartões inteligentes. Portanto, o AD FS fornece uma interface de usuário do lado do cliente (UI) para obter credenciais PIN (número) de identificação pessoal do cartão inteligente. Isso ocorre porque os clientes baseados em Windows intencionalmente não fornecem detalhes de credencial do usuário para os servidores de Federação ou servidores Web.  
  
### <a name="smart-card-authentication"></a>Autenticação com cartão inteligente  
Autenticação de cartão inteligente usa o protocolo Kerberos para autenticar em um servidor de federação de conta. O AD FS não pode ser estendido para adicionar novos métodos de autenticação. O certificado do cartão inteligente não é exigido para o encadeamento até uma raiz confiável no computador cliente. Usar um certificado baseado em cartão inteligente com o AD FS requer as seguintes condições:  
  
-   O leitor e o CSP (provedor de serviços de criptografia) do cartão inteligente devem funcionar no computador onde o navegador está localizado.  
  
-   O certificado de cartão inteligente deve se liga a uma raiz confiável no servidor de federação de conta e o proxy de servidor de federação de conta.  
  
-   O certificado deve ser associado à conta de usuário no AD DS por um dos seguintes métodos:  
  
    -   O nome da entidade do certificado corresponde ao nome diferenciado LDAP de uma conta de usuário no AD DS.  
  
    -   A extensão do nome alternativa da entidade do certificado tem o nome UPN de uma conta de usuário no AD DS.  
  
Para dar suporte a certos requisitos de força de autenticação em alguns cenários, também é possível configurar o AD FS a fim de criar uma declaração que indique como o usuário foi autenticado. Uma terceira parte confiável pode usar essa declaração para tomar uma decisão sobre a autorização.  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
