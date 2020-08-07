---
title: bitsadmin setcredentials
description: Artigo de referência do comando Bitsadmin SetCredentials, que adiciona credenciais a um trabalho.
ms.topic: article
ms.assetid: 3cd099a4-9e85-46d8-8527-edb6dfab7f97
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c13c2bd544c485bef59858e7262169a66d1fc94d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893206"
---
# <a name="bitsadmin-setcredentials"></a>bitsadmin setcredentials

Adiciona credenciais a um trabalho.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,2 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setcredentials <job> <target> <scheme> <username> <password>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| destino | Use o **servidor** ou **proxy**. |
| scheme | Use um dos seguintes:<ul><li>**Basic.** Esquema de autenticação em que o nome de usuário e a senha são enviados em texto não criptografado para o servidor ou proxy.</li><li>**Digest.** Um esquema de autenticação de desafio/resposta que usa uma cadeia de caracteres de dados especificada pelo servidor para o desafio.</li><li>**NTLM.** Um esquema de autenticação de desafio/resposta que usa as credenciais do usuário para autenticação em um ambiente de rede do Windows.</li><li>**NEGOTIAte (também conhecido como protocolo de negociação simples e protegido).** Um esquema de autenticação de desafio/resposta que negocia com o servidor ou proxy para determinar qual esquema usar para autenticação. Os exemplos são o protocolo Kerberos e NTLM.</li><li>**Passaporte.** Um serviço de autenticação centralizado fornecido pela Microsoft que oferece um logon único para sites membros.</li></ul> |
| user_name | O nome de usuário. |
| password | A senha associada ao nome de *usuário*fornecido. |

## <a name="examples"></a>Exemplos

Para adicionar credenciais ao trabalho chamado *myDownloadJob*:

```
bitsadmin /setcredentials myDownloadJob SERVER BASIC Edward password20
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
