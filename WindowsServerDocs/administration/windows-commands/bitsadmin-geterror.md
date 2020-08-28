---
title: bitsadmin geterror
description: Artigo de referência para o comando GetError Bitsadmin, que recupera informações de erro detalhadas para o trabalho especificado.
ms.topic: reference
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7ebb554b269dd31ce96943097c7888a8836580b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030354"
---
# <a name="bitsadmin-geterror"></a>bitsadmin geterror

Recupera informações detalhadas de erro para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /geterror <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar as informações de erro para o trabalho chamado *myDownloadJob*:

```
bitsadmin /geterror myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
