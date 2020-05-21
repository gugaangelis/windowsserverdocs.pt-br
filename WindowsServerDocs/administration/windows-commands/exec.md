---
title: exec
description: Tópico de referência para o comando exec, que executa um arquivo de script no computador local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 956f3d4a7c5992980aea0fc0f5933ee7def48381
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436102"
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
