---
title: bitsadmin gettemporaryname
description: Artigo de referência para o comando gettemporárioname do Bitsadmin, que relata o nome de arquivo temporário do arquivo fornecido dentro do trabalho.
ms.topic: reference
ms.assetid: 68925edc-a801-4292-a812-7471c4f60fdd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9eeaf3c1093cf147c945bb029e5652db1819003c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631593"
---
# <a name="bitsadmin-gettemporaryname"></a>bitsadmin gettemporaryname

Informa o nome de arquivo temporário do arquivo fornecido dentro do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /gettemporaryname <job> <file_index>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| file_index | Começa em 0. |

## <a name="examples"></a>Exemplos

Para relatar o nome de arquivo temporário de File 2 para o trabalho chamado *myDownloadJob*:

```
bitsadmin /gettemporaryname myDownloadJob 1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
