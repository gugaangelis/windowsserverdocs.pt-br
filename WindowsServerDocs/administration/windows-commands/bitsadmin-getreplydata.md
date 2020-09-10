---
title: bitsadmin getreplydata
description: Artigo de referência para o comando Bitsadmin getreplydata, que recupera os dados de upload-resposta do servidor em formato hexadecimal para o trabalho.
ms.topic: reference
ms.assetid: 819f97d5-b255-4b2d-9f63-0daa73915434
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: a15dc59d9487ed928954f8d5cbd47828c2b58291
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631727"
---
# <a name="bitsadmin-getreplydata"></a>bitsadmin getreplydata

Recupera os dados de resposta de upload do servidor em formato hexadecimal para o trabalho.

> [!NOTE]
> Esse comando não tem suporte no BITS 1,2 e versões anteriores.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getreplydata <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar os dados de resposta de upload para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getreplydata myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
