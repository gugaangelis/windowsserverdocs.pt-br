---
ms.assetid: 39ecc468-77c5-4938-827e-48ce498a25ad
title: "Apêndice A - examinando os requisitos do AD FS"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: d5cfb5de77843eebfc152b9c79ac55bab1fa7727
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-a-reviewing-ad-fs-requirements"></a>Apêndice a: examinando os requisitos do AD FS

>Aplica-se a: Windows Server 2012

Para que os parceiros organizacionais na sua implementação de serviços de Federação do Active Directory (AD FS) podem colaborar com êxito, você deverá primeiro verificar se sua infraestrutura de rede corporativa está configurada para dar suporte a requisitos do AD FS para contas, resolução de nomes e certificados. AD FS tem os seguintes tipos de requisitos:  
  
> [!TIP]  
> Você pode encontrar links adicionais de recurso do AD FS no [mapa de conteúdo do AD FS](https://social.technet.microsoft.com/wiki/contents/articles/2735.aspx) página no Wiki do Microsoft TechNet. Esta página é gerenciada por membros da comunidade AD FS e monitorada regularmente pela equipe de produto do AD FS.  
  
## <a name="hardware-requirements"></a>Requisitos de hardware  
Se aplicam aos seguintes requisitos mínimos e recomendados de hardware para o servidor de Federação e computadores de proxy do servidor de Federação.  
  
|Requisito de hardware|Requisito mínimo|Requisito recomendado|  
|------------------------|-----------------------|---------------------------|  
|Velocidade da CPU|Dual-core, 1 gigahertz (GHz)|Processador Quad-core, 2 GHz|  
|RAM|1 GB|4 GB|  
|Espaço em disco|50 MB|100 MB|  
  
## <a name="software-requirements"></a>Requisitos de software  
AD FS depende da funcionalidade de servidor que é integrada ao sistema operacional Windows Server® 2012.  
  
> [!NOTE]  
> Os serviços de função do serviço de Federação e Proxy de serviço de Federação não podem coexistir no mesmo computador.  
  
## <a name="certificate-requirements"></a>Requisitos de certificado  
Certificados reproduzir a função mais importante na proteção das comunicações entre servidores de federação, proxies do servidor de federação, aplicativos compatíveis com declarações e clientes da Web. Requisitos de certificados variam, dependendo se você estiver configurando um servidor de Federação ou o computador de proxy do servidor de federação, conforme descrito nesta seção.  
  
### <a name="federation-server-certificates"></a>Certificados de servidor de Federação  
Servidores de Federação exigem os certificados na tabela a seguir.  
  
|Tipo de certificado|Descrição|O que você precisa saber antes de implantar|  
|--------------------|---------------|------------------------------------------|  
|Seguro de certificados Sockets Layer (SSL)|Isso é um certificado Secure Sockets Layer (SSL) padrão que é usado para proteger as comunicações entre clientes e servidores de Federação.|Esse certificado deve ser vinculado ao Site da Web padrão no Internet Information Services (IIS) para um servidor de Federação ou um Proxy de servidor de Federação.  Para um Proxy de servidor de federação, a associação deve ser configurada no IIS antes de executar o Assistente de configuração de Proxy de servidor de federação com êxito.<br /><br />**Recomendação:** porque esse certificado deve ser confiável pelos clientes do AD FS, use um certificado de autenticação de servidor é emitido por uma autoridade de certificação pública de (terceiro) (CA), por exemplo, VeriSign. **Dica:** o nome do requerente do certificado é usado para representar o nome do serviço de federação para cada instância do AD FS que você implantar. Por esse motivo, convém considerar a escolher um nome de assunto em qualquer novo recebem certificados que melhor represente o nome da sua empresa ou organização para parceiros.|  
|Certificado de comunicação do serviço|Esse certificado permite que a segurança de mensagem WCF para proteger as comunicações entre servidores de Federação.|Por padrão, o certificado SSL é usado como o certificado de comunicações de serviço.  Isso pode ser alterado usando o console de gerenciamento do AD FS.|  
|Certificado de assinatura de token|Este é um padrão X509 certificado usado para assinar com segurança todos os tokens que o servidor de Federação emite.|O certificado de assinatura de token deve conter uma chave privada e ele deve encadear em uma raiz confiável no serviço de Federação. Por padrão, o AD FS cria um certificado autoassinado. No entanto, você pode alterar isso posteriormente para um certificado emitido pela autoridade de certificação usando o snap-in do AD FS gerenciamento, dependendo das necessidades da sua organização.|  
|Certificado de descriptografia de token|Isso é um certificado SSL padrão que é usado para descriptografar qualquer tokens de entrada que são criptografados por um servidor de Federação do parceiro. Ele também é publicado nos metadados de Federação.|Por padrão, o AD FS cria um certificado autoassinado. No entanto, você pode alterar isso posteriormente para um certificado emitido pela autoridade de certificação usando o snap-in do AD FS gerenciamento, dependendo das necessidades da sua organização.|  
  
> [!CAUTION]  
> Certificados que são usados para o token de assinatura e descriptografar o token são fundamentais para a estabilidade do serviço de Federação. Como uma perda ou não planejada remoção de todos os certificados que são configuradas para essa finalidade pode interromper o serviço, você deve fazer backup de todos os seus certificados que são configurados para essa finalidade.  
  
Para saber mais sobre os certificados usados por servidores de federação, consulte [requisitos de certificado para servidores de Federação](Certificate-Requirements-for-Federation-Servers.md).  
  
### <a name="federation-server-proxy-certificates"></a>Certificados de proxy do servidor de Federação  
Proxies de servidor de Federação exigem os certificados na tabela a seguir.  
  
|Tipo de certificado|Descrição|O que você precisa saber antes de implantar|  
|--------------------|---------------|------------------------------------------|  
|Certificado de autenticação de servidor|Isso é um certificado Secure Sockets Layer (SSL) padrão que é usado para proteger as comunicações entre um proxy do servidor de Federação e um cliente de Internet computadores.|Esse certificado deve ser vinculado ao Site da Web padrão no Internet Information Services (IIS) antes de executar o Assistente de configuração do AD FS federação servidor Proxy com êxito.<br /><br />**Recomendação:** porque esse certificado deve ser confiável pelos clientes do AD FS, use um certificado de autenticação de servidor é emitido por uma autoridade de certificação pública de (terceiro) (CA), por exemplo, VeriSign.<br /><br />**Dica:** o nome do requerente do certificado é usado para representar o nome do serviço de federação para cada instância do AD FS que você implantar. Por esse motivo, convém considerar a escolher um nome de entidade que melhor represente o nome da sua empresa ou organização para parceiros.|  
  
Para saber mais sobre os certificados que usam proxies de servidor de federação, consulte [requisitos de certificado para Proxies de servidor de Federação](Certificate-Requirements-for-Federation-Server-Proxies.md).  
  
## <a name="browser-requirements"></a>Requisitos de navegador  
Embora qualquer navegador da Web atual com a funcionalidade de JavaScript pode ser feita para funcionar como um cliente do AD FS, páginas da Web que são fornecidas por padrão foram testadas apenas em versões do Internet Explorer 7.0, 8.0 e 9.0, Mozilla Firefox 3.0 e Safari 3.1 no Windows. JavaScript deve estar habilitado e cookies devem estar ativado para baseada em navegador entrada e saída para funcionar corretamente.  
  
A equipe de produto do AD FS na Microsoft testado com êxito as configurações do navegador e sistema operacional na tabela a seguir.  
  
|Navegador|Windows 7|Windows Vista|  
|-----------|-------------|-----------------|  
|Internet Explorer 7.0|X|X|  
|O Internet Explorer 8.0|X|X|  
|O Internet Explorer 9.0|X|Não testado.|  
|FireFox 3.0|X|X|  
|Safari 3.1|X|X|  
  
> [!NOTE]  
> AD FS dá suporte a versões de 32 bits tanto o 64bit de todos os navegadores mostrando na tabela acima.  
  
### <a name="cookies"></a>Cookies  
AD FS cria cookies persistentes e com base em sessão que devem ser armazenados em computadores cliente para fornecer entrada, saída single sign-on (SSO) e outras funcionalidades. Portanto, o navegador do cliente deve ser configurado para aceitar cookies. Cookies são usados para autenticação sempre são os cookies de sessão Secure Hypertext Transfer Protocol (HTTPS) que são escritos para o servidor de origem. Se o navegador cliente não estiver configurado para permitir que esses cookies, AD FS não funcionará corretamente. Cookies persistentes são usados para preservar a seleção do usuário do provedor de declarações. Você pode desativá-los usando uma configuração no arquivo de configuração para as páginas de entrada do AD FS.  
  
Suporte a TLS/SSL é necessário por motivos de segurança.  
  
## <a name="network-requirements"></a>Requisitos de rede  
Configurar os seguintes serviços de rede adequadamente é essencial para a implantação bem-sucedida do AD FS em sua organização.  
  
### <a name="tcpip-network-connectivity"></a>Conectividade de rede TCP/IP  
AD FS funcione, conectividade de rede TCP/IP deve existir entre o cliente; um controlador de domínio; e os computadores que hospedam o serviço de federação, o Proxy de serviço de Federação (quando ele é usado) e o agente de Web do AD FS.  
  
### <a name="dns"></a>DNS  
O serviço de rede principal que é crítico para a operação do AD FS, diferente de serviços de domínio do Active Directory (AD DS), é o sistema de nome de domínio (DNS). Quando o DNS é implantado, os usuários podem usar nomes de computadores amigáveis que são fáceis de lembrar para se conectar a computadores e outros recursos em redes IP.  
  
 Windows Server 2008 usa o DNS para resolução de nome em vez da resolução de nome NetBIOS do serviço de cadastramento na Internet do Windows (WINS) que foi usada em redes baseados em Windows NT 4.0. Ele ainda é possível usar o WINS para aplicativos que precisam dele. No entanto, o AD DS e do AD FS exigem a resolução de nomes DNS.  
  
O processo de configuração DNS para dar suporte a AD FS varia dependendo se:  
  
-   Sua organização já tem uma infraestrutura DNS existente. Na maioria dos cenários, DNS já está configurado em toda a sua rede para que os clientes de navegador da Web em sua rede corporativa tenham acesso à Internet. Como resolução de acesso e o nome da Internet são os requisitos do AD FS, presume-se que essa infraestrutura para estar em vigor para a implantação do AD FS.  
  
-   Você pretende adicionar um servidor federado à sua rede corporativa. Para fins de autenticação de usuários na rede corporativa, servidores DNS internos na floresta rede corporativa devem ser configurados para retornar o CNAME do servidor interno que está executando o serviço de Federação. Para obter mais informações, consulte [requisitos de resolução de nome para servidores de Federação](Name-Resolution-Requirements-for-Federation-Servers.md).  
  
-   Você pretende adicionar um proxy server federado à sua rede do perímetro. Quando você quiser autenticam contas de usuário que estão localizadas na rede corporativa da sua organização do parceiro de identidade, os servidores DNS internos na floresta rede corporativa devem ser configurados para retornar o CNAME de proxy do servidor de Federação interno. Para obter informações sobre como configurar o DNS para acomodar a adição de proxies de servidor de federação, consulte [requisitos de resolução de nome de Proxies de servidor de Federação](Name-Resolution-Requirements-for-Federation-Server-Proxies.md).  
  
-   Você está configurando o DNS para um ambiente de laboratório de teste. Se você pretende usar o AD FS em um ambiente de laboratório de teste onde não há servidor DNS de raiz único é autoritativo, é provável que você precisa configurar encaminhadores DNS, de forma que consultas para nomes entre dois ou mais florestas serão encaminhadas adequadamente. Para obter informações gerais sobre como configurar um ambiente de laboratório de teste do AD FS, consulte [AD FS passo a passo e como nas guias](https://go.microsoft.com/fwlink/?LinkId=180357).  
  
## <a name="attribute-store-requirements"></a>Atributo requisitos da loja  
AD FS requer pelo menos um repositório de atributo a ser usado para autenticar usuários e extrair declarações de segurança para esses usuários. Para uma lista de atributo armazena AD FS dá suporte, consulte [função The do atributo armazena](../../ad-fs/technical-reference/The-Role-of-Attribute-Stores.md) no guia de Design do AD FS.  
  
> [!NOTE]  
> AD FS cria automaticamente um repositório de atributo do Active Directory, por padrão.  
  
Requisitos de loja atributo dependem se sua organização está atuando como o parceiro de conta (hospedagem os usuários federados) ou o parceiro de recurso (hospeda o aplicativo federado).  
  
### <a name="ad-ds"></a>AD DS  
Do AD FS operar com êxito, controladores de domínio na organização do parceiro de conta ou a organização de parceiro de recurso devem estar executando o Windows Server 2003 SP1, Windows Server 2003 R2, Windows Server 2008 ou Windows Server 2012.  
  
Quando o AD FS é instalado e configurado em um computador ingressado no domínio, o repositório de conta de usuário no Active Directory para esse domínio é disponibilizado como um repositório de atributo selecionáveis.  
  
> [!IMPORTANT]  
> Como o AD FS requer a instalação do Internet Information Services (IIS), recomendamos que você não instale o software do AD FS em um controlador de domínio em um ambiente de produção para fins de segurança. No entanto, essa configuração é suportada pelo serviço de atendimento da Microsoft.  
  
#### <a name="schema-requirements"></a>Requisitos de esquema  
AD FS não exige alterações no esquema ou modificações de nível funcional no AD DS.  
  
#### <a name="functional-level-requirements"></a>Requisitos de nível funcional  
A maioria dos recursos do AD FS não exigem modificações de nível funcional do AD DS para operar com êxito. No entanto, funcional do domínio do Windows Server 2008 nível ou superior é necessária para autenticação de certificado de cliente operar com êxito se o certificado é mapeado para uma conta do usuário no AD DS.  
  
#### <a name="service-account-requirements"></a>Requisitos de conta de serviço  
Se você estiver criando um farm de servidores de federação, primeiro você deve criar uma conta de serviço baseado em domínio dedicado no AD DS que pode usar o serviço de Federação. Mais tarde, você deve configurar cada servidor de federação no farm para usar essa conta. Para saber mais sobre como fazer isso, consulte [manualmente configurar uma conta de serviço para um Farm de servidores de Federação](../../ad-fs/deployment/Manually-Configure-a-Service-Account-for-a-Federation-Server-Farm.md) no guia de implantação do AD FS.  
  
### <a name="ldap"></a>LDAP  
Quando você trabalha com outras lojas com base em LDAP Lightweight Directory Access Protocol atributo, você deve se conectar a um servidor LDAP que oferece suporte à autenticação integrada do Windows. A cadeia de caracteres de conexão LDAP também deve ser gravada no formato de uma URL LDAP, conforme descrito em RFC 2255.  
  
### <a name="sql-server"></a>SQL Server  
Do AD FS operar com êxito, os computadores que hospedam o repositório de atributo de servidor de linguagem de consulta estruturada (SQL) devem estar executando o Microsoft SQL Server 2005 ou SQL Server 2008. Quando você trabalha com o atributo baseado em SQL lojas, você também deve configurar uma cadeia de caracteres de conexão.  
  
### <a name="custom-attribute-stores"></a>Repositórios de atributo personalizado  
Você pode desenvolver repositórios de atributo personalizado para habilitar cenários avançados. O idioma de política é integrado ao AD FS pode fazer referência a repositórios de atributo personalizado para que qualquer um dos cenários a seguir podem ser aperfeiçoados:  
  
-   Criando declarações para um usuário autenticado localmente  
  
-   Complementando declarações para um usuário autenticado externamente  
  
-   Autorizar um usuário obtenha um token  
  
-   Autorizar um serviço para obter um token no comportamento de um usuário  
  
Quando você trabalha com um repositório de atributo personalizado, você também terá que configurar uma cadeia de caracteres de conexão. Nessa situação, você pode inserir qualquer código personalizado desejar que permite que uma conexão para o repositório de atributo personalizado. A cadeia de caracteres de conexão nesta situação é um conjunto de pares nome/valor que são interpretados como implementado pelo desenvolvedor do repositório de atributo personalizado.  
  
Para obter mais informações sobre como desenvolver e usando o atributo personalizado lojas, consulte [visão geral do atributo da Store](https://go.microsoft.com/fwlink/?LinkId=190782).  
  
## <a name="application-requirements"></a>Requisitos de aplicativo  
Servidores de federação podem se comunicar com e proteger aplicativos de federação, como aplicativos compatíveis com declarações.  
  
## <a name="authentication-requirements"></a>Requisitos de autenticação  
AD FS integra-se naturalmente com a autenticação do Windows existente, por exemplo, a autenticação Kerberos, NTLM, cartões inteligentes e certificados de cliente do x. 509 v3. Servidores de federação usam autenticação Kerberos padrão para autenticar um usuário em um domínio. Os clientes podem autenticar usando autenticação baseada em formulários, autenticação de cartão inteligente e autenticação integrada do Windows, dependendo de como você configura a autenticação.  
  
A função de proxy do servidor de Federação AD FS possibilita um cenário em que o usuário se autentica externamente usando autenticação SSL de clientes. Você também pode configurar a função de servidor de federação para exigir autenticação de cliente SSL, embora normalmente a melhor experiência do usuário é obtido por meio da configuração do servidor de federação de conta para autenticação integrada do Windows. Nessa situação, o AD FS não tem controle sobre quais credenciais que o usuário emprega para logon da área de trabalho do Windows.  
  
### <a name="smart-card-logon"></a>Logon com cartão inteligente  
Embora o AD FS pode impor o tipo de credenciais que ele usa para autenticação (senhas, autenticação de cliente SSL ou autenticação integrada do Windows), ele não impõe diretamente autenticação com cartões inteligentes. Portanto, o AD FS não fornece uma interface do usuário do lado do cliente (IU) para obter credenciais PIN (número) de identificação pessoal do cartão inteligente. Isso ocorre porque clientes baseados em Windows intencionalmente não fornecem detalhes de credenciais de usuário para servidores de Federação ou servidores Web.  
  
### <a name="smart-card-authentication"></a>Autenticação de cartão inteligente  
Autenticação de cartão inteligente usa o protocolo Kerberos para autenticar a um servidor de federação de conta. AD FS não pode ser estendido para adicionar novos métodos de autenticação. O certificado de cartão inteligente não é necessário para cadeia até uma raiz confiável no computador cliente. O uso de um certificado com base em cartão inteligente com o AD FS requer as seguintes condições:  
  
-   O leitor e o provedor de serviços de criptografia (CSP) para o cartão inteligente devem funcionar no computador onde o navegador está localizado.  
  
-   O certificado de cartão inteligente deve encadear em uma raiz confiável no servidor de federação de conta e o proxy de servidor de federação de conta.  
  
-   O certificado deve mapear para a conta de usuário no AD DS por qualquer um dos seguintes métodos:  
  
    -   Nome do requerente do certificado corresponde ao nome diferenciado LDAP de uma conta de usuário no AD DS.  
  
    -   A extensão de altname de assunto do certificado tem o nome principal do usuário (UPN) de uma conta de usuário no AD DS.  
  
Para dar suporte a determinados requisitos de intensidade de autenticação em alguns cenários, também é possível configurar o AD FS para criar uma declaração que indica como o usuário foi autenticado. Um terceiro pode usar esta declaração para tomar uma decisão de autorização.  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
