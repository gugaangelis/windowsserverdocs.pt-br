---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: Configurar o AD FS para autenticar usuários armazenados nos diretórios do LDAP
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2053f0a93f33cdfdd85eec8cdbb6eca4ebad1ff0
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444923"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories"></a>Configurar o AD FS para autenticar usuários armazenados nos diretórios do LDAP

O tópico a seguir descreve a configuração necessária para habilitar a infraestrutura do AD FS autenticar os usuários cujas identidades são armazenadas em diretórios compatíveis com v3 do Lightweight Directory Access Protocol (LDAP).

Em muitas organizações, as soluções de gerenciamento de identidade consistem em uma combinação de Active Directory, AD LDS ou diretórios LDAP de terceiros. Com a adição de suporte do AD FS para autenticar usuários armazenados em diretórios LDAP v3 compatível, você pode se beneficiar de empresarial todo conjunto independentemente de onde suas identidades de usuário são armazenadas de recursos do AD FS. O AD FS dá suporte a diretórios LDAP v3 compatível.

> [!NOTE]
> Alguns dos recursos do AD FS incluir logon único (SSO), autenticação de dispositivos, políticas de acesso condicional flexível, suporte para o trabalho-de-em qualquer lugar por meio da integração com o Proxy de aplicativo Web e contínuo federação com o Azure AD, que por sua vez permite que você e seus usuários utilizar a nuvem, incluindo o Office 365 e outros aplicativos SaaS.  Para obter mais informações, consulte [visão geral do Active Directory Federation Services](../../ad-fs/AD-FS-2016-Overview.md).

Em ordem para o AD FS autenticar usuários de um diretório LDAP, você deve se conectar neste diretório LDAP para seu farm do AD FS com a criação de um **relação de confiança de provedor de declarações local**.  Uma relação de confiança do provedor de declarações local é um objeto de confiança que representa um diretório LDAP no seu farm do AD FS. Objeto consiste em uma variedade de identificadores, nomes e regras para identificar esse diretório LDAP para o serviço de federação local de confiança do provedor de declarações de um local.

Você pode dar suporte a vários diretórios LDAP, cada um com sua própria configuração, dentro do mesmo farm do AD FS, adicionando vários **relações de confiança de provedor de declarações local**. Além disso, as florestas do AD DS que não são confiáveis para a floresta do AD FS residente na também podem ser modeladas como objetos de confiança de provedor de declarações local. Você pode criar relações de confiança de provedor de declarações local usando o Windows PowerShell.

Diretórios LDAP (relações de confiança de provedor de declarações local) podem coexistir com diretórios do AD (relações de confiança de provedor de declarações) no mesmo servidor do AD FS, dentro do mesmo farm do AD FS, portanto, uma única instância do AD FS é capaz de autenticar e autorizar o acesso para usuários que estão não AD e armazenados no AD diretórios.

Somente a autenticação baseada em formulários há suporte para autenticação de usuários de diretórios LDAP. Não há suporte para a autenticação do Windows integrada e baseada em certificado para autenticar usuários em diretórios LDAP.

Todos os protocolos de autorização passivo que são suportados pelo AD FS, incluindo SAML, WS-Federation e OAuth também têm suporte para identidades que são armazenadas em diretórios LDAP.

Também há suporte para o protocolo de autorização do Active Directory do WS-Trust para identidades que são armazenadas em diretórios LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurar o AD FS para autenticar usuários armazenados em um diretório LDAP
Para configurar o farm do AD FS para autenticar usuários em um diretório LDAP, você pode concluir as etapas a seguir:

1. Primeiro, configure uma conexão para o diretório LDAP usando o **New-AdfsLdapServerConnection** cmdlet:

   ```
   $DirectoryCred = Get-Credential
   $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
   ```

   > [!NOTE]
   > É recomendável que você crie um novo objeto de conexão para cada servidor LDAP que você deseja se conectar. AD FS pode conectar vários servidores LDAP de réplica e automaticamente o failover no caso de um servidor específico do LDAP está inoperante. Para tais casos, você pode criar um AdfsLdapServerConnection para cada um desses servidores LDAP de réplica e, em seguida, adicionar a matriz de objetos de conexão usando-**LdapServerConnection** parâmetro do  **Adicionar-AdfsLocalClaimsProviderTrust** cmdlet.

   **OBSERVAÇÃO:** A tentativa de usar o Get-Credential e digite um DN e senha a ser usado para associar a uma instância LDAP pode resultar em uma falha porque do requisito de interface do usuário para formatos de entrada específicos, por exemplo, domínio \ nomedeusuário ou user@domain.tld. Em vez disso, você pode usar o cmdlet ConvertTo-SecureString da seguinte maneira (o exemplo a seguir pressupõe que a uid = admin, UO = sistema, como o DN das credenciais a ser usado para associar a instância do LDAP):

   ```
   $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
   $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
   ```

   Em seguida, insira a senha para a uid = admin e conclua o restante das etapas.

2. Em seguida, você pode executar a etapa opcional de mapeamento de atributos LDAP para as declarações do AD FS existentes usando o **New-AdfsLdapAttributeToClaimMapping** cmdlet. No exemplo a seguir, você mapeia o nome, sobrenome, LDAP CommonName atributos e para as declarações do AD FS:

   ```
   #Map given name claim
   $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
   # Map surname claim
   $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
   # Map common name claim
   $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
   ```

   Esse mapeamento é feito para disponibilizar atributos da loja LDAP como declarações no AD FS para criar regras de controle de acesso condicional no AD FS. Ele também permite que o AD FS trabalhar com esquemas personalizados em repositórios de LDAP, fornecendo uma maneira fácil para mapear atributos LDAP para declarações.

3. Por fim, você deve registrar o repositório de LDAP com o AD FS, como um local de declaração de confiança do provedor usando o **AdfsLocalClaimsProviderTrust adicionar** cmdlet:

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

   No exemplo acima, você está criando uma relação de confiança de provedor de declarações local chamada "Fornecedores". Você está especificando informações de conexão para o AD FS para se conectar ao diretório LDAP que essa relação de confiança do provedor de declarações local representa atribuindo `$vendorDirectory` para o `-LdapServerConnection` parâmetro. Observe que, na etapa um, que você atribuiu `$vendorDirectory` uma cadeia de caracteres de conexão a ser usado ao conectar-se ao diretório LDAP específico. Por fim, você está especificando que o `$GivenName`, `$Surname`, e `$CommonName` atributos LDAP (que é mapeado para as declarações do AD FS) devem ser usados para controle de acesso condicional, incluindo as políticas de autenticação multifator e emissão regras de autorização, bem como para emissão por meio de declarações em tokens de segurança emitido pelo FS do AD. Para usar protocolos como Ws-Trust de Active Directory com o AD FS, você deve especificar o parâmetro de OrganizationalAccountSuffix, que permite que o AD FS para resolver a ambiguidade entre declarações locais confianças do provedor ao executar o serviço uma solicitação de autorização do Active Directory.

## <a name="see-also"></a>Consulte também
[Operações do AD FS](../../ad-fs/AD-FS-2016-Operations.md)


