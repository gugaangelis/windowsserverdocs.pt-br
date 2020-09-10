---
title: bitsadmin gethelpertokenflags
description: Artigo de referência para o comando Bitsadmin gethelpertokenflags, que retorna os sinalizadores de uso para um token auxiliar que está associado a um trabalho de transferência de BITS.
ms.topic: reference
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 03/01/2019
ms.openlocfilehash: 244dee15e201c877bdf3c17a219e190bee9a5821
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632056"
---
# <a name="bitsadmin-gethelpertokenflags"></a>bitsadmin gethelpertokenflags

Retorna os sinalizadores de uso para um [token auxiliar](/windows/win32/bits/helper-tokens-for-bits-transfer-jobs)   que está associado a um trabalho de transferência de bits.

> [!NOTE]
> Esse comando não tem suporte no BITS 3,0 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /gethelpertokenflags <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

### <a name="remarks"></a>Comentários

Possíveis valores de retorno, incluindo:

- **0x0001.** O token auxiliar é usado para abrir o arquivo local de um trabalho de upload, para criar ou renomear o arquivo temporário de um trabalho de download ou para criar ou renomear o arquivo de resposta de um trabalho de resposta de upload.

- **0x0002.** O token auxiliar é usado para abrir o arquivo remoto de um trabalho de upload ou download do protocolo SMB ou em resposta a um servidor HTTP ou a um desafio de proxy para credenciais NTLM ou Kerberos implícitas. Você deve chamar  `/SetCredentialsJob TargetScheme NULL NULL`   para permitir que as credenciais sejam enviadas via http.

## <a name="examples"></a>Exemplos

Para recuperar os sinalizadores de uso para um token auxiliar associado a um trabalho de transferência de BITS chamado *myDownloadJob*:

```
bitsadmin /gethelpertokenflags myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
