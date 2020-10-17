---
title: ver
description: Artigo de referência para o comando ver, que exibe o número de versão do sistema operacional.
ms.topic: reference
ms.assetid: 5a9c6cd4-b67d-4b30-8c56-5f9798eafd2a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9e99c5af9ee45b33cb0c050307c83c89874d89cb
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156277"
---
# <a name="ver"></a>ver

Exibe o número de versão do sistema operacional. Esse comando tem suporte no prompt de comando do Windows (Cmd.exe), mas não no PowerShell.

## <a name="syntax"></a>Sintaxe

```
ver
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | DESCRIÇÃO |
|--|--|
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para obter o número de versão do sistema operacional do Shell de comando (cmd.exe), digite:

```
ver
```

O comando **Ver** não funciona no PowerShell. Se você quiser obter o número de versão do sistema operacional por meio do PowerShell, digite:

```powershell
$PSVersionTable.BuildVersion
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
