---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: "Configurar o AD FS para autenticar usuários armazenados nos diretórios LDAP"
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 05f8b8991e664a84c3f2b3200de4068af8d1476a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>Configurar o AD FS para autenticar usuários armazenados nos diretórios LDAP

>Aplica-se a: Windows Server 2016

O tópico a seguir descreve a configuração necessária para habilitar sua infraestrutura do AD FS autenticar os usuários cujas identidades são armazenadas em diretórios compatíveis com v3 Lightweight Directory Access Protocol (LDAP).

Em muitas organizações, soluções de gerenciamento de identidade consistem em uma combinação do Active Directory, AD LDS ou diretórios LDAP de terceiros. Com a adição de suporte do AD FS para autenticar usuários armazenados em diretórios compatíveis com v3 LDAP, você pode se beneficiar do inteiro corporativo AD FS recurso conjunto não importa onde suas identidades do usuário são armazenadas. AD FS dá suporte a qualquer diretório compatível com v3 LDAP.

> [!NOTE]
> Alguns dos recursos do AD FS incluem o logon único (SSO), autenticação de dispositivo, políticas de acesso condicional flexível, suporte para o trabalho-da-em qualquer lugar por meio de integração com o Proxy de aplicativo Web e federação contínua com o Azure AD que por sua vez, permite que você e os usuários utilizam a nuvem, incluindo Office 365 e outros aplicativos SaaS.  Para obter mais informações, consulte [visão geral do Active Directory federação serviços ](../../ad-fs/AD-FS-2016-Overview.md).

Na ordem do AD FS autenticar os usuários em um diretório LDAP, você deve conectar esse diretório LDAP seu farm AD FS através da criação de um **confiança de provedor local requerimentos judiciais ou Extrajudiciais **.  Uma relação de confiança do provedor de declarações local é um objeto de confiança que representa um diretório LDAP no farm AD FS. Um local declarações de confiança do provedor objeto consiste em uma variedade de identificadores, nomes e as regras que identifica esse diretório LDAP ao serviço de federação local.

Você pode dar suporte a vários diretórios LDAP, cada uma com sua própria configuração, dentro do mesmo farm AD FS adicionando várias **relações de confiança de provedor local requerimentos judiciais ou Extrajudiciais **. Além disso, florestas do AD DS que não sejam confiáveis pela floresta do AD FS vive na também podem ser estabelecidas como relações de confiança de provedor de declarações local. Você pode criar relações de confiança de provedor de declarações local usando o Windows PowerShell.

Diretórios LDAP (relações de confiança de provedor local requerimentos judiciais ou Extrajudiciais) podem coexistir com diretórios do AD (relações de confiança de provedor de declarações) no mesmo servidor do AD FS, dentro do mesmo farm AD FS, portanto, uma única instância do AD FS é capaz de autenticar e autorizar o acesso para os usuários que estão armazenadas nos anúncios e anúncios não são diretórios.

Autenticação baseada em formulários só há suporte para autenticação de usuários de diretórios LDAP. Não há suporte para autenticação baseada em certificado e integradas do Windows para autenticar usuários em diretórios LDAP.

Todos os protocolos de autorização passivo que são suportados pelo AD FS, incluindo SAML, WS-Federation e OAuth também têm suporte para identidades que são armazenadas em diretórios LDAP.

Também é compatível com o protocolo de autorização active WS-Trust identidades que são armazenadas em diretórios LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurar o AD FS para autenticar usuários armazenados em um diretório LDAP
Para configurar seu farm AD FS para autenticar os usuários em um diretório LDAP, você pode concluir as etapas a seguir:

1.  Primeiro, configure uma conexão para o diretório LDAP usando o **nova AdfsLdapServerConnection** cmdlet:

    ```
    $DirectoryCred = Get-Credential
    $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
    ```

    > [!NOTE]
    > É recomendável que você crie um novo objeto de conexão para cada servidor LDAP que você deseja se conectar. AD FS pode se conectar a vários servidores LDAP réplica e automaticamente o failover no caso de um servidor LDAP específico estiver desligado. Para o caso, você pode criar um AdfsLdapServerConnection para cada um desses servidores de LDAP réplica e, em seguida, adicione a matriz de objetos de conexão usando as opções -**LdapServerConnection** parâmetro do **AdfsLocalClaimsProviderTrust adicionar** cmdlet.

    **Observação:** a tentativa de usar Get-Credential e digite um DN e uma senha para ser usada para associar a uma instância LDAP pode resultar em uma falha porque o sobre a necessidade de interface do usuário de formatos específicos de entrada, por exemplo, domínio \ usuário ou user@domain.tld. Em vez disso, você pode usar o cmdlet ConvertTo-SecureString da seguinte maneira (o exemplo a seguir presume uid = admin, UO = sistema como o DN das credenciais a ser usada para associar a instância do LDAP):

    ```
    $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
    $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
    ```

    Insira a senha para o x:UID = admin e preencha o restante das etapas.

2.  Em seguida, você pode executar as etapas opcionais de mapeamento de atributos LDAP para as declarações do AD FS existentes usando o **nova AdfsLdapAttributeToClaimMapping** cmdlet. No exemplo a seguir, você mapear givenName, sobrenome, e CommonName LDAP atributos para as declarações do AD FS:

    ```
    #Map given name claim
    $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
    # Map surname claim
    $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
    # Map common name claim
    $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
    ```

    Esse mapeamento é feito para disponibilizar atributos do repositório de dados LDAP como declarações no AD FS para criar regras de controle de acesso condicional no AD FS. Ele habilita também o AD FS trabalhar com esquemas personalizados em lojas LDAP, fornecendo uma maneira fácil de atributos LDAP são mapeadas para declarações.

3.  Por fim, você deve registrar o repositório LDAP com o AD FS como um local declarações do provedor de confiança usando o **AdfsLocalClaimsProviderTrust adicionar** cmdlet:

    ```
    Add-AdfsLocalClaimsProviderTrust -Name "Vendors" -Identifier "urn:vendors" -Type Ldap

    # Connection info
    -LdapServerConnection $vendorDirectory 

    # How to locate user objects in directory
    -UserObjectClass inetOrgPerson -UserContainer "CN=VendorsContainer,CN=VendorsPartition" -LdapAuthenticationMethod Basic 

    # Claims for authenticated users
    -AnchorClaimLdapAttribute mail -AnchorClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/upn" -LdapAttributeToClaimMapping @($GivenName, $Surname, $CommonName) 

    # General claims provider properties
    -AcceptanceTransformRules "c:[Type != ''] => issue(claim=c);" -Enabled $true 

    # Optional - supply user name suffix if you want to use Ws-Trust
    -OrganizationalAccountSuffix "vendors.contoso.com"

    ```

    No exemplo acima, você está criando uma relação de confiança de provedor local requerimentos judiciais ou Extrajudiciais chamada "Fornecedores". Você especifica as informações de conexão do AD FS para se conectar ao diretório LDAP esta relação de confiança do provedor local requerimentos judiciais ou Extrajudiciais representa atribuindo `$vendorDirectory`para o `-LdapServerConnection`parâmetro. Observe que, na etapa 1, que você atribuiu `$vendorDirectory`uma cadeia de caracteres de conexão para ser usada durante a conexão ao diretório LDAP específico. Por fim, você especifica que o `$GivenName`, `$Surname`, e `$CommonName`atributos LDAP (que você mapeada para as declarações do AD FS) devem ser usadas para controle de acesso condicional, incluindo a autenticação multifator políticas e regras de autorização de emissão, bem como para emissão via declarações em tokens de segurança emitido FS do AD. Para usar protocolos active como Ws-Trust com o AD FS, você deve especificar o parâmetro OrganizationalAccountSuffix, que permite que o AD FS remover a ambiguidade entre declarações local provedor relações de confiança ao fazer a manutenção de uma solicitação de autorização active.

## <a name="see-also"></a>Consulte também
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)


