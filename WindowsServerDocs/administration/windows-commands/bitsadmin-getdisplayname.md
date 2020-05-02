---
title: bitsadmin getdisplayname
description: Tópico de referência para o comando GetDisplayName do Bitsadmin, que recupera o nome de exibição do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e5c0e76c-4cc6-42d8-ac30-30bf3dc11b9b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f7c92fdb7c743c1a4c71f076764f5d1da2d95a6f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718024"
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
