---
title: Manage-bde pausar
description: Artigo de referência para o comando de pausa Manage-bde, que pausa a criptografia ou descriptografia do BitLocker.
ms.topic: reference
ms.assetid: efda0e08-b9ff-4e71-83d8-bb666b3032bd
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ca15aa1b48e0b06a036eaf8906c7d5b7d43b881b
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639643"
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
| -ComputerName | Especifica que manage-bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
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
