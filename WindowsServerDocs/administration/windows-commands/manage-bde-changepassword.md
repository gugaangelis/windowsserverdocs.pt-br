---
title: ChangePassword de Manage-bde
description: Tópico de referência para o comando ChangePassword Manage-bde, que modifica a senha para uma unidade de dados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b174e152-8442-4fba-8b33-56a81ff4f547
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 28cc97165bfc33809c187630e37ad9b9bd24d7c6
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222924"
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
| -ComputerName | Especifica que o Manage-bde. exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
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
