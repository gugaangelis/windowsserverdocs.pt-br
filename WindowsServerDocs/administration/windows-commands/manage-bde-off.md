---
title: Manage-bde desativado
description: Tópico de referência para o comando Manage-bde off, que descriptografa a unidade e desativa o BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0a27c119-d385-45e5-89fe-e311d4429876
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cbcec0cadba870a5f416af50f12e0b2c5eed6d95
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222861"
---
# <a name="manage-bde-off"></a>Manage-bde desativado

Descriptografa a unidade e desativa o BitLocker. Todos os protetores de chave são removidos quando a descriptografia é concluída.

## <a name="syntax"></a>Sintaxe

```
manage-bde -off [<volume>] [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Para desativar o BitLocker na unidade C, digite:

```
manage-bde –off C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Manage-bde no comando](manage-bde-on.md)

- [comando de pausa Manage-bde](manage-bde-pause.md)

- [comando de retomada Manage-bde](manage-bde-resume.md)

- [comando Manage-bde](manage-bde.md)
