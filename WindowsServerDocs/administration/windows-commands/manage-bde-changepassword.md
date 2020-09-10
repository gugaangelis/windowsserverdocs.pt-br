---
title: ChangePassword de Manage-bde
description: Artigo de referência para o comando ChangePassword Manage-bde, que modifica a senha de uma unidade de dados.
ms.topic: reference
ms.assetid: b174e152-8442-4fba-8b33-56a81ff4f547
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 8c262b6ca123d76af6ebdca09e5546f41350686d
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622703"
---
# <a name="manage-bde-changepassword"></a>ChangePassword de Manage-bde

Modifica a senha de uma unidade de dados. O usuário é solicitado a fornecer uma nova senha.

## <a name="syntax"></a>Sintaxe

```
manage-bde -changepassword [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Para alterar a senha usada para desbloquear o BitLocker na unidade de dados D, digite:

```
manage-bde –changepassword D:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
