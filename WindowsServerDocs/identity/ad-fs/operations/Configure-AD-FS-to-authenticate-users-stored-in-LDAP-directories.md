---
ms.assetid: e863ab80-4e4c-48d3-bdaa-31815ef36bae
title: Configurar o AD FS para autenticar usuários armazenados nos diretórios do LDAP
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.openlocfilehash: 95dfeb427aa67bce56c3f87f2c356eff34aac6c4
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87962500"
---
# <a name="configure-ad-fs-to-authenticate-users-stored-in-ldap-directories-in-windows-server-2016-or-later"></a>Configurar AD FS para autenticar usuários armazenados em diretórios LDAP no Windows Server 2016 ou posterior

O tópico a seguir descreve a configuração necessária para habilitar sua infraestrutura de AD FS para autenticar usuários cujas identidades são armazenadas em diretórios compatíveis com o protocolo LDAP v3.

Em muitas organizações, as soluções de gerenciamento de identidades consistem em uma combinação de Active Directory, AD LDS ou diretórios LDAP de terceiros. Com a adição de suporte de AD FS para autenticar usuários armazenados em diretórios compatíveis com LDAP v3, você pode se beneficiar de todo o conjunto de recursos de AD FS de nível empresarial, independentemente de onde suas identidades de usuário estão armazenadas. O AD FS dá suporte a qualquer diretório compatível com LDAP v3.

> [!NOTE]
> Alguns dos recursos de AD FS incluem SSO (logon único), autenticação de dispositivo, políticas de acesso condicional flexíveis, suporte para trabalho de qualquer lugar por meio da integração com o proxy de aplicativo Web e Federação direta com o Azure AD que, por sua vez, permite que você e seus usuários utilizem a nuvem, incluindo o Office 365 e outros aplicativos SaaS.  Para obter mais informações, consulte [serviços de Federação do Active Directory (AD FS) visão geral](../ad-fs-overview.md).

Para AD FS autenticar usuários de um diretório LDAP, você deve conectar esse diretório LDAP ao farm de AD FS criando uma **relação de confiança do provedor de declarações local**.  Uma relação de confiança do provedor de declarações local é um objeto de confiança que representa um diretório LDAP em seu farm de AD FS. Um objeto de confiança do provedor de declarações local consiste em uma variedade de identificadores, nomes e regras que identificam esse diretório LDAP para o serviço de federação local.

Você pode dar suporte a vários diretórios LDAP, cada um com sua própria configuração, dentro do mesmo farm de AD FS adicionando várias **relações de confiança de provedor de declarações locais**. Além disso, AD DS florestas que não são confiáveis para a floresta em que AD FS residem também podem ser modeladas como relações de confiança do provedor de declarações local. Você pode criar relações de confiança do provedor de declarações locais usando o Windows PowerShell.

Os diretórios LDAP (relações de confiança do provedor de declarações locais) podem coexistir com diretórios do AD (confianças do provedor de declarações) no mesmo servidor de AD FS, dentro do mesmo farm de AD FS, portanto, uma única instância do AD FS é capaz de autenticar e autorizar o acesso para usuários armazenados em diretórios AD e não AD.

Somente a autenticação baseada em formulários tem suporte para autenticar usuários de diretórios LDAP. Não há suporte para autenticação integrada do Windows e com base em certificado para autenticar usuários em diretórios LDAP.

Todos os protocolos de autorização passivos com suporte pelo AD FS, incluindo SAML, WS-Federation e OAuth também têm suporte para identidades que são armazenadas em diretórios LDAP.

O protocolo WS-Trust active Authorization também tem suporte para identidades que são armazenadas em diretórios LDAP.

## <a name="configure-ad-fs-to-authenticate-users-stored-in-an-ldap-directory"></a>Configurar AD FS para autenticar usuários armazenados em um diretório LDAP
Para configurar o farm de AD FS para autenticar usuários de um diretório LDAP, você pode concluir as seguintes etapas:

1. Primeiro, configure uma conexão com o diretório LDAP usando o cmdlet **New-AdfsLdapServerConnection** :

   ```
   $DirectoryCred = Get-Credential
   $vendorDirectory = New-AdfsLdapServerConnection -HostName dirserver -Port 50000 -SslMode None -AuthenticationMethod Basic -Credential $DirectoryCred
   ```

   > [!NOTE]
   > É recomendável que você crie um novo objeto de conexão para cada servidor LDAP ao qual deseja se conectar. AD FS pode se conectar a vários servidores LDAP de réplica e fazer failover automaticamente caso um servidor LDAP específico esteja inativo. Para esse caso, você pode criar um AdfsLdapServerConnection para cada um desses servidores LDAP de réplica e, em seguida, adicionar a matriz de objetos de conexão usando o parâmetro-**LdapServerConnection** do cmdlet **Add-AdfsLocalClaimsProviderTrust** .

   **Observação:** A tentativa de usar Get-Credential e digitar um DN e uma senha a ser usada para associar a uma instância LDAP pode resultar em uma falha devido ao requisito de interface do usuário para formatos de entrada específicos, por exemplo, domínio \ nomedeusuário ou user@domain.tld . Em vez disso, você pode usar o cmdlet ConvertTo-SecureString da seguinte maneira (o exemplo abaixo pressupõe que UID = admin, ou = System como o DN das credenciais a serem usadas para ligar à instância LDAP):

   ```
   $ldapuser = ConvertTo-SecureString -string "uid=admin,ou=system" -asplaintext -force
   $DirectoryCred = Get-Credential -username $ldapuser -Message "Enter the credentials to bind to the LDAP instance:"
   ```

   Em seguida, insira a senha para o UID = admin e conclua o restante das etapas.

2. Em seguida, você pode executar a etapa opcional de mapear atributos LDAP para as declarações de AD FS existentes usando o cmdlet **New-AdfsLdapAttributeToClaimMapping** . No exemplo abaixo, você mapeia os atributos LDAPname, sobrenome e CommonName para as declarações de AD FS:

   ```
   #Map given name claim
   $GivenName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute givenName -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/givenname"
   # Map surname claim
   $Surname = New-AdfsLdapAttributeToClaimMapping -LdapAttribute sn -ClaimType "http://schemas.xmlsoap.org/ws/2005/05/identity/claims/surname"
   # Map common name claim
   $CommonName = New-AdfsLdapAttributeToClaimMapping -LdapAttribute cn -ClaimType "http://schemas.xmlsoap.org/claims/CommonName"
   ```

   Esse mapeamento é feito para tornar os atributos do armazenamento LDAP disponíveis como declarações em AD FS para criar regras de controle de acesso condicional no AD FS. Ele também permite que AD FS funcionem com esquemas personalizados em armazenamentos LDAP, fornecendo uma maneira fácil de mapear atributos LDAP para declarações.

3. Por fim, você deve registrar o repositório LDAP com AD FS como uma relação de confiança do provedor de declarações local usando o cmdlet **Add-AdfsLocalClaimsProviderTrust** :

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

   No exemplo acima, você está criando uma relação de confiança do provedor de declarações local chamada "Vendors". Você está especificando informações de conexão para AD FS se conectar ao diretório LDAP que essa confiança do provedor de declarações local representa, atribuindo `$vendorDirectory` ao `-LdapServerConnection` parâmetro. Observe que, na etapa 1, você atribuiu `$vendorDirectory` uma cadeia de conexão a ser usada ao conectar-se ao seu diretório LDAP específico. Finalmente, você está especificando que os `$GivenName` `$Surname` atributos LDAP,, e `$CommonName` (que você mapeou para as declarações de AD FS) devem ser usados para o controle de acesso condicional, incluindo políticas de autenticação multifator e regras de autorização de emissão, bem como para a emissão por meio de declarações em tokens de segurança emitidos por AD FS. Para usar protocolos ativos como o WS-Trust com AD FS, você deve especificar o parâmetro OrganizationalAccountSuffix, que permite AD FS a ambiguidade entre as relações de confiança do provedor de declarações locais ao atender a uma solicitação de autorização ativa.

## <a name="see-also"></a>Consulte Também
[Operações do AD FS](../ad-fs-operations.md)
