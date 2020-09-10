---
title: gerenciar o bloqueio do bde
description: Artigo de referência para o comando de bloqueio Manage-bde, que bloqueia uma unidade protegida pelo BitLocker para impedir o acesso a ela, a menos que a chave de desbloqueio seja fornecida.
ms.topic: reference
ms.assetid: b8858e61-3a7e-4d03-8c98-5c09853f35e8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e9b8dfdac7bc8b833a89c9ba447b99984a923f86
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633743"
---
# <a name="manage-bde-lock"></a>gerenciar o bloqueio do bde

Bloqueia uma unidade protegida pelo BitLocker para impedir o acesso a ela, a menos que a chave de desbloqueio seja fornecida.

## <a name="syntax"></a>Sintaxe

```
manage-bde -lock [<drive>] [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Para bloquear a unidade de dados D, digite:

```
manage-bde –lock D:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)
