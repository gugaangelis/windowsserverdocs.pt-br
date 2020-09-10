---
title: Manage-bde desativado
description: Artigo de referência para o comando Manage-bde off, que descriptografa a unidade e desativa o BitLocker.
ms.topic: reference
ms.assetid: 0a27c119-d385-45e5-89fe-e311d4429876
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8802b971b6d63e6f0693b6b40222c325ab576bc3
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639652"
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
| -ComputerName | Especifica que manage-bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
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
