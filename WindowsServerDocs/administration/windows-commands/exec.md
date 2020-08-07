---
title: exec
description: Artigo de referência para o comando exec, que executa um arquivo de script no computador local.
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7d0fd297ca3b5908a6782379dbd716098e47f751
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87890482"
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
