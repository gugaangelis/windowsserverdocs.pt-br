---
title: exec
description: Artigo de referência para o comando exec, que executa um arquivo de script no computador local.
ms.topic: reference
ms.assetid: 364e8baf-576f-401b-a431-7d3c06621614
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 668dc3cf3d93d11066d7dece4309f586b3a5b964
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635967"
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
