---
title: Manage-bde ChangeKey
description: Artigo de referência para o comando Manage-bde ChangeKey, que modifica a chave de inicialização para uma unidade do sistema operacional.
ms.topic: reference
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 3752d0a3a990488129b99a5ebbe44e830552c00f
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622746"
---
# <a name="manage-bde-changekey"></a>Manage-bde ChangeKey

Modifica a chave de inicialização para uma unidade do sistema operacional.

## <a name="syntax"></a>Sintaxe

```
manage-bde -changekey [<drive>] [<pathtoexternalkeydirectory>] [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<drive>` | Representa uma letra de unidade seguida de dois-pontos. |
| -ComputerName | Especifica que manage-bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

### <a name="examples"></a>Exemplos

Para criar uma nova chave de inicialização na unidade E, para usar com a criptografia BitLocker na unidade C, digite:

```
manage-bde -changekey C: E:\
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
