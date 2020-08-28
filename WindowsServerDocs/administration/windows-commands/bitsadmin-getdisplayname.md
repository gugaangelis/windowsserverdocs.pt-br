---
title: bitsadmin getdisplayname
description: Artigo de referência para o comando GetDisplayName do Bitsadmin, que recupera o nome de exibição do trabalho especificado.
ms.topic: reference
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bb953125ade98ecc158d61e53ea7f4462c455b15
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030383"
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
