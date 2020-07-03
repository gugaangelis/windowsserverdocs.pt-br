---
title: exec
description: Artigo de referência para o comando exec, que executa um arquivo de script no computador local.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b4c9ff71b886bd84120bd3af7c1f8d8c143425da
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932308"
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
