---
title: bitsadmin removecredentials
description: Artigo de referência para o comando Bitsadmin removecredentials, que remove as credenciais de um trabalho.
ms.topic: reference
ms.assetid: 4a78ce9a-1feb-4811-a000-cce81287b22b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4db9ddba312aba0d0fdd1dc63cbbf3c964afd043
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89026334"
---
# <a name="bitsadmin-removecredentials"></a>bitsadmin removecredentials

Remove as credenciais de um trabalho.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,2 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /removecredentials <job> <target> <scheme>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| destino | Use o **servidor** ou **proxy**. |
| scheme | Use um dos seguintes:<ul><li>**Basic.** Esquema de autenticação em que o nome de usuário e a senha são enviados em texto não criptografado para o servidor ou proxy.</li><li>**Digest.** Um esquema de autenticação de desafio/resposta que usa uma cadeia de caracteres de dados especificada pelo servidor para o desafio.</li><li>**NTLM.** Um esquema de autenticação de desafio/resposta que usa as credenciais do usuário para autenticação em um ambiente de rede do Windows.</li><li>**NEGOTIAte (também conhecido como protocolo de negociação simples e protegido).** Um esquema de autenticação de desafio/resposta que negocia com o servidor ou proxy para determinar qual esquema usar para autenticação. Os exemplos são o protocolo Kerberos e NTLM.</li><li>**Passaporte.** Um serviço de autenticação centralizado fornecido pela Microsoft que oferece um logon único para sites membros.</li></ul> |

## <a name="examples"></a>Exemplos

Para remover as credenciais do trabalho chamado *myDownloadJob*:

```
bitsadmin /removecredentials myDownloadJob SERVER BASIC
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
