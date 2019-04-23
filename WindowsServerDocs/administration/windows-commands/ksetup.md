---
title: ksetup
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0194cf81d069d7a5c1223f0a514d593e4870d397
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868837"
---
# <a name="ksetup"></a>ksetup



Executa tarefas relacionadas para configurar e manter o protocolo Kerberos e o Centro de distribuição de chaves (KDC) para dar suporte a realms Kerberos, que também não são domínios do Windows. Para obter exemplos de como esse comando pode ser usado, consulte a seção de exemplos em cada um dos subtópicos relacionados.

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

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[Ksetup:setrealm](ksetup-setrealm.md)|Torna esse computador um membro de um realm Kerberos.|
|[Ksetup:mapuser](ksetup-mapuser.md)|Mapeia uma entidade de segurança do Kerberos para uma conta.|
|[Ksetup:addkdc](ksetup-addkdc.md)|Define uma entrada do KDC para o realm especificado.|
|[Ksetup:delkdc](ksetup-delkdc.md)|Exclui uma entrada do KDC para o realm.|
|[Ksetup:addkpasswd](ksetup-addkpasswd.md)|Adiciona um endereço de servidor Kpasswd para um território.|
|[Ksetup:delkpasswd](ksetup-delkpasswd.md)|Exclui um endereço de servidor Kpasswd para um território.|
|[Ksetup:server](ksetup-server.md)|Permite que você especifique o nome de um computador Windows no qual aplicar as alterações.|
|[Ksetup:setcomputerpassword](ksetup-setcomputerpassword.md)|Define a senha para a conta de domínio do computador (ou entidade de segurança do host).|
|[Ksetup:removerealm](ksetup-removerealm.md)|Exclui todas as informações para o realm especificado do registro.|
|[Ksetup:domain](ksetup-domain.md)|Permite que você especifique um domínio (se \<DomainName > não foi definido por meio **/domain**).|
|[Ksetup:changepassword](ksetup-changepassword.md)|Permite que você use o Kpasswd para alterar a senha do usuário conectado.|
|[Ksetup:listrealmflags](ksetup-listrealmflags.md)|Lista as disponíveis realm sinaliza que **ksetup** pode detectar.|
|[Ksetup:setrealmflags](ksetup-setrealmflags.md)|Define os sinalizadores de realm para um território específico.|
|[Ksetup:addrealmflags](ksetup-addrealmflags.md)|Adiciona os sinalizadores de território adicionais para um território.|
|[Ksetup:delrealmflags](ksetup-delrealmflags.md)|Exclui os sinalizadores de território de um território.|
|[Ksetup:dumpstate](ksetup-dumpstate.md)|Analisa a configuração do Kerberos em determinado computador. Adiciona um host para o mapeamento de realm para o registro.|
|[Ksetup:addhosttorealmmap](ksetup-addhosttorealmmap.md)|Adiciona um valor do registro para mapear o host para o realm do Kerberos.|
|[Ksetup:delhosttorealmmap](ksetup-delhosttorealmmap.md)|Exclui o valor do registro que mapeado o computador host para o realm do Kerberos.|
|[Ksetup:setenctypeattr](ksetup-setenctypeattr.md)|Define os atributos de confiança para o domínio de um ou mais tipos de criptografia.|
|[Ksetup:getenctypeattr](ksetup-getenctypeattr.md)|Obtém o atributo de relação de confiança de tipos de criptografia para o domínio.|
|[Ksetup:addenctypeattr](ksetup-addenctypeattr.md)|Adiciona tipos de criptografia para o atributo de relação de confiança de tipos de criptografia para o domínio.|
|[Ksetup:delenctypeattr](ksetup-delenctypeattr.md)|Exclui o atributo de relação de confiança de tipos de criptografia para o domínio.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

**Ksetup** é usado para alterar as configurações do computador para localizar os realms Kerberos. Em implementações de não-Microsoft – baseada em Kerberos, essas informações geralmente são mantidas no arquivo Krb5.conf. Em sistemas operacionais Windows Server, ele é mantido no registro. Você pode usar essa ferramenta para modificar essas configurações. Essas configurações são usadas por estações de trabalho para localizar os realms Kerberos e controladores de domínio para localizar os realms Kerberos para relações de confiança entre territórios.

**Ksetup** inicializa as chaves do registro que o provedor de suporte de segurança (SSP) Kerberos usa para localizar um KDC para o realm do Kerberos se o computador está executando o Windows Server 2003, Windows Server 2008 ou Windows Server 2008 R2 e não é um membro de um Windows domínio. Após a configuração, o de um computador cliente que está executando o Windows no sistema operacional pode fazer logon em contas de usuário no realm Kerberos.

O protocolo Kerberos versão 5 é o padrão para autenticação de rede em computadores que executam o Windows XP Professional, Windows Vista e Windows 7. O SSP do Kerberos procura o registro para o nome de domínio do território do usuário e, em seguida, resolve o nome para um endereço IP ao consultar um servidor DNS. O protocolo Kerberos pode usar o DNS para localizar os KDCs usando apenas o nome do território, mas ele deve ser configurado especialmente para fazer isso.

#### <a name="additional-references"></a>Referências adicionais

-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)