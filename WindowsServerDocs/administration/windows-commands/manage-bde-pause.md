---
title: Manage-bde pausar
description: Tópico de referência para o comando de pausa Manage-bde, que pausa a criptografia ou descriptografia do BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: efda0e08-b9ff-4e71-83d8-bb666b3032bd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8eaa28d6be61c5a06698a39be02cd20ff8b72a8
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222841"
---
# <a name="manage-bde-pause"></a>Manage-bde pausar

Pausa a criptografia ou descriptografia do BitLocker.

## <a name="syntax"></a>Sintaxe

```
manage-bde -pause [<volume>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<volume>` | Especifica uma letra de unidade seguida por dois-pontos, um caminho de GUID de volume ou um volume montado. |
| -ComputerName | Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

### <a name="examples"></a>Exemplos

Para pausar a criptografia BitLocker na unidade C, digite:

```
manage-bde pause C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Manage-bde no comando](manage-bde-on.md)

- [comando Manage-bde off](manage-bde-off.md)

- [comando de retomada Manage-bde](manage-bde-resume.md)

- [comando Manage-bde](manage-bde.md)
