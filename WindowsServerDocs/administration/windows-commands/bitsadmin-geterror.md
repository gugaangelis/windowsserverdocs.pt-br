---
title: bitsadmin geterror
description: Tópico de referência para o comando GetError Bitsadmin, que recupera informações de erro detalhadas para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cbe5bca1-d2dd-4ce6-903f-f85de4a2ec6a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7275bb813ef65f304cf8111c5a1ee387b7528e5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718012"
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
