---
title: bitsadmin getdisplayname
description: Artigo de referência para o comando GetDisplayName do Bitsadmin, que recupera o nome de exibição do trabalho especificado.
ms.topic: reference
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2e106f9b1815d735502ee451ec59c7f34f9ea063
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632133"
---
# <a name="bitsadmin-getdisplayname"></a>bitsadmin getdisplayname

Recupera o nome de exibição do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getdisplayname <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o nome de exibição para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getdisplayname myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
