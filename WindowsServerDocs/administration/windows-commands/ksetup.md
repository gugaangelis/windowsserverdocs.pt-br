---
title: ksetup
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2f3fde0ada4ab8bcbe52eccf22b959f99f91319f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724546"
---
# <a name="ksetup"></a>ksetup



Executa tarefas relacionadas à configuração e à manutenção do protocolo Kerberos e do centro de distribuição de chaves (KDC) para dar suporte a territórios Kerberos, que não são também domínios do Windows. Para obter exemplos de como esse comando pode ser usado, consulte a seção exemplos em cada um dos subtópicos relacionados.

## <a name="syntax"></a>Sintaxe

```
ksetup 
[/setrealm <DNSDomainName>] 
[/mapuser <Principal> <Account>] 
[/addkdc <RealmName> <KDCName>] 
[/delkdc <RealmName> <KDCName>]
[/addkpasswd <RealmName> <KDCPasswordName>] 
[/delkpasswd <RealmName> <KDCPasswordName>]
[/server <ServerName>] 
[/setcomputerpassword <Password>]
[/removerealm <RealmName>]  
[/domain <DomainName>] 
[/changepassword <OldPassword> <NewPassword>] 
[/listrealmflags] 
[/setrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/addrealmflags <RealmName> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/delrealmflags [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]] 
[/dumpstate]
[/addhosttorealmmap] <HostName> <RealmName>]  
[/delhosttorealmmap] <HostName> <RealmName>]  
[/setenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/getenctypeattr] <DomainName>
[/addenctypeattr] <DomainName> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/delenctypeattr] <DomainName>

```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Ksetup:setrealm](ksetup-setrealm.md)|Torna este computador um membro de um realm Kerberos.|
|[Ksetup:mapuser](ksetup-mapuser.md)|Mapeia uma entidade de segurança Kerberos para uma conta.|
|[Ksetup:addkdc](ksetup-addkdc.md)|Define uma entrada KDC para o realm especificado.|
|[Ksetup:delkdc](ksetup-delkdc.md)|Exclui uma entrada KDC para o realm.|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|Adiciona um endereço de servidor Kpasswd para um realm.|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|Exclui um endereço de servidor Kpasswd para um realm.|
|[Ksetup:server](ksetup-server.md)|Permite especificar o nome de um computador Windows no qual as alterações serão aplicadas.|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|Define a senha para a conta de domínio do computador (ou a entidade de segurança do host).|
|[Ksetup:removerealm](ksetup-removerealm.md)|Exclui todas as informações do realm especificado do registro.|
|[Ksetup:domain](ksetup-domain.md)|Permite que você especifique um domínio (se \<o domainname> não tiver sido definido usando **/Domain**).|
|[Ksetup:changepassword](ksetup-changepassword.md)|Permite que você use o Kpasswd para alterar a senha do usuário conectado.|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|Lista os sinalizadores de realm disponíveis que o **ksetup** pode detectar.|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|Define os sinalizadores de realm para um realm específico.|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|Adiciona sinalizadores de realm adicionais a um realm.|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|Exclui os sinalizadores de realm de um realm.|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|Analisa a configuração do Kerberos no computador especificado. Adiciona um host ao mapeamento de realm para o registro.|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|Adiciona um valor de registro para mapear o host para o realm do Kerberos.|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|Exclui o valor do registro que mapeou o computador host para o realm do Kerberos.|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|Define um ou mais tipos de criptografia atributos de confiança para o domínio.|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|Obtém o atributo de confiança dos tipos de criptografia para o domínio.|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|Adiciona tipos de criptografia ao atributo de confiança tipos de criptografia para o domínio.|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|Exclui o atributo de confiança dos tipos de criptografia para o domínio.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

**Ksetup** é usado para alterar as configurações do computador para localizar territórios Kerberos. Em implementações não Microsoft baseadas em Kerberos, essas informações geralmente são mantidas no arquivo krb5. conf. Em sistemas operacionais Windows Server, ele é mantido no registro. Você pode usar essa ferramenta para modificar essas configurações. Essas configurações são usadas pelas estações de trabalho para localizar territórios Kerberos e por controladores de domínio para localizar territórios Kerberos para relações de confiança entre territórios.

**Ksetup** Inicializa chaves do registro que o SSP (provedor de suporte de segurança) do Kerberos usa para localizar um KDC para o realm do Kerberos se o computador estiver executando o windows Server 2003, o windows Server 2008 ou o windows Server 2008 R2 e não for membro de um domínio do Windows. Após a configuração, o usuário de um computador cliente que está executando o sistema operacional Windows pode fazer logon em contas no realm do Kerberos.

O protocolo Kerberos versão 5 é o padrão para autenticação de rede em computadores que executam o Windows XP Professional, o Windows Vista e o Windows 7. O SSP do Kerberos pesquisa no registro o nome de domínio do realm do usuário e, em seguida, resolve o nome para um endereço IP consultando um servidor DNS. O protocolo Kerberos pode usar o DNS para localizar KDCs usando apenas o nome do Realm, mas ele deve ser configurado especialmente para fazer isso.

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)