---
title: gerenciar o desbloqueio automático do bde
description: Artigo de referência para o comando autolock do Manage-bde, que gerencia o desbloqueio automático de unidades de dados protegidas pelo BitLocker.
ms.topic: reference
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: e6035b9d4a851631770b01ba525759d1d61992f5
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89633752"
---
# <a name="manage-bde-autounlock"></a>gerenciar o desbloqueio automático do bde

Gerencia o desbloqueio automático de unidades de dados protegidas pelo BitLocker.

## <a name="syntax"></a>Sintaxe

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -habilitar | Habilita o desbloqueio automático para uma unidade de dados. |
| -desabilitar | Desabilita o desbloqueio automático de uma unidade de dados. |
| -clearallkeys | Remove todas as chaves externas armazenadas na unidade do sistema operacional. |
| `<drive>` | Representa uma letra de unidade seguida de dois-pontos. |
| -ComputerName | Especifica que manage-bde.exe será usado para modificar a proteção do BitLocker em um computador diferente. Você também pode usar **-CN** como uma versão abreviada desse comando. |
| `<name>` | Representa o nome do computador no qual a proteção do BitLocker será modificada. Os valores aceitos incluem o nome NetBIOS do computador e o endereço IP do computador. |
| -? ou/? | Exibe a ajuda resumida no prompt de comando. |
| -Help ou-h | Exibe a ajuda completa no prompt de comando. |

## <a name="examples"></a>Exemplos

Para habilitar o desbloqueio automático da unidade de dados E, digite:

```
manage-bde –autounlock -enable E:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)