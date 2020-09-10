---
title: Manage-bde changepin
description: Artigo de referência para o comando Manage-bde changepin, que modifica o PIN de uma unidade do sistema operacional.
ms.topic: reference
ms.assetid: c85aa1c7-3485-4839-a292-99dfcd6db252
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7b920e9c4580fced678e9d7dddd30fff66dad802
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622632"
---
# <a name="manage-bde-changepin"></a>Manage-bde changepin

Modifica o PIN de uma unidade do sistema operacional. É solicitado que o usuário insira um novo PIN.

## <a name="syntax"></a>Sintaxe

```
manage-bde -changepin [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Para alterar o PIN usado com o BitLocker na unidade C, digite:

```
manage-bde –changepin C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
