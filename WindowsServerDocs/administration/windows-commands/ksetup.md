---
title: ksetup
description: Artigo de referência para o comando ksetup, que executa tarefas relacionadas à configuração e à manutenção do protocolo Kerberos e do centro de distribuição de chaves (KDC) para dar suporte a territórios Kerberos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 4e046f8a-811b-48dc-9a69-18d8e097f353
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a0398d53516f81de68a7de5854ed2c996a78d1e5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922630"
---
# <a name="ksetup"></a>ksetup

Executa tarefas relacionadas à configuração e à manutenção do protocolo Kerberos e do centro de distribuição de chaves (KDC) para dar suporte a territórios Kerberos. Especificamente, esse comando é usado para:

- Altere as configurações do computador para localizar territórios Kerberos. Em implementações não Microsoft, baseadas em Kerberos, essas informações geralmente são mantidas no arquivo krb5. conf. Em sistemas operacionais Windows Server, ele é mantido no registro. Você pode usar essa ferramenta para modificar essas configurações. Essas configurações são usadas pelas estações de trabalho para localizar territórios Kerberos e por controladores de domínio para localizar territórios Kerberos para relações de confiança entre territórios.

- Inicialize as chaves do registro que o SSP (provedor de suporte de segurança) do Kerberos usará para localizar um KDC para o realm do Kerberos, se o computador não for um membro de um domínio do Windows. Após a configuração, o usuário de um computador cliente que executa o sistema operacional Windows pode fazer logon em contas no realm do Kerberos.

- Pesquise no registro o nome de domínio do realm do usuário e, em seguida, resolva o nome para um endereço IP consultando um servidor DNS. O protocolo Kerberos pode usar o DNS para localizar KDCs usando apenas o nome do Realm, mas ele deve ser configurado especialmente para fazer isso.

## <a name="syntax"></a>Sintaxe

```
ksetup
[/setrealm <DNSdomainname>]
[/mapuser <principal> <account>]
[/addkdc <realmname> <KDCname>]
[/delkdc <realmname> <KDCname>]
[/addkpasswd <realmname> <KDCPasswordName>]
[/delkpasswd <realmname> <KDCPasswordName>]
[/server <servername>]
[/setcomputerpassword <password>]
[/removerealm <realmname>]
[/domain <domainname>]
[/changepassword <oldpassword> <newpassword>]
[/listrealmflags]
[/setrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/addrealmflags <realmname> [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/delrealmflags [sendaddress] [tcpsupported] [delegate] [ncsupported] [rc4]]
[/dumpstate]
[/addhosttorealmmap] <hostname> <realmname>]
[/delhosttorealmmap] <hostname> <realmname>]
[/setenctypeattr] <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/getenctypeattr] <domainname>
[/addenctypeattr] <domainname> {DES-CBC-CRC | DES-CBC-MD5 | RC4-HMAC-MD5 | AES128-CTS-HMAC-SHA1-96 | AES256-CTS-HMAC-SHA1-96}
[/delenctypeattr] <domainname>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| [ksetup setrealm](ksetup-setrealm.md) | Torna este computador um membro de um realm Kerberos. |
| [ksetup addkdc](ksetup-addkdc.md) | Define uma entrada KDC para o realm especificado. |
| [ksetup delkdc](ksetup-delkdc.md) | Exclui uma entrada KDC para o realm. |
| [ksetup addkpasswd](ksetup-addkpasswd.md) | Adiciona um endereço de servidor kpasswd para um realm. |
| [ksetup delkpasswd](ksetup-delkpasswd.md) | Exclui um endereço de servidor kpasswd para um realm. |
| [ksetup server](ksetup-server.md) | Permite especificar o nome de um computador Windows no qual as alterações serão aplicadas. |
| [ksetup setcomputerpassword](ksetup-setcomputerpassword.md) | Define a senha para a conta de domínio do computador (ou a entidade de segurança do host). |
| [ksetup removerealm](ksetup-removerealm.md) | Exclui todas as informações do realm especificado do registro. |
| [ksetup domain](ksetup-domain.md) | Permite que você especifique um domínio (se ainda não tiver `<domainname>` sido definido pelo parâmetro **/Domain** ). |
| [ksetup changepassword](ksetup-changepassword.md) | Permite que você use o kpasswd para alterar a senha do usuário conectado. |
| [ksetup listrealmflags](ksetup-listrealmflags.md) | Lista os sinalizadores de realm disponíveis que o **ksetup** pode detectar. |
| [ksetup setrealmflags](ksetup-setrealmflags.md) | Define os sinalizadores de realm para um realm específico. |
| [ksetup addrealmflags](ksetup-addrealmflags.md) | Adiciona sinalizadores de realm adicionais a um realm. |
| [ksetup delrealmflags](ksetup-delrealmflags.md) | Exclui os sinalizadores de realm de um realm. |
| [ksetup dumpstate](ksetup-dumpstate.md) | Analisa a configuração do Kerberos no computador especificado. Adiciona um host ao mapeamento de realm para o registro. |
| [ksetup addhosttorealmmap](ksetup-addhosttorealmmap.md) | Adiciona um valor de registro para mapear o host para o realm do Kerberos. |
| [ksetup delhosttorealmmap](ksetup-delhosttorealmmap.md) | Exclui o valor do registro que mapeou o computador host para o realm do Kerberos. |
| [ksetup setenctypeattr](ksetup-setenctypeattr.md) | Define um ou mais tipos de criptografia atributos de confiança para o domínio. |
| [ksetup getenctypeattr](ksetup-getenctypeattr.md) | Obtém o atributo de confiança dos tipos de criptografia para o domínio. |
| [ksetup addenctypeattr](ksetup-addenctypeattr.md) | Adiciona tipos de criptografia ao atributo de confiança tipos de criptografia para o domínio. |
| [ksetup delenctypeattr](ksetup-delenctypeattr.md) | Exclui o atributo de confiança dos tipos de criptografia para o domínio. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)