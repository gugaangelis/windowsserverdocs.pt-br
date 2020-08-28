---
title: exec
description: Artigo de referência para o comando exec, que executa um arquivo de script no computador local.
ms.topic: reference
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e0e4cd876fe5c6abcf909f20e330ff347db1113b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89023990"
---
# <a name="exec"></a>exec

Executa um arquivo de script no computador local. Esse comando também duplica ou restaura os dados como parte de uma sequência de backup ou restauração. Se o script falhar, um erro será retornado e o DiskShadow sairá.

O arquivo pode ser um script **cmd** .

## <a name="syntax"></a>Sintaxe

```
exec <scriptfile.cmd>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<scriptfile.cmd>` | Especifica o arquivo de script a ser executado. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando DiskShadow](diskshadow.md)
