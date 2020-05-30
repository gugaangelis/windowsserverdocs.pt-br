---
title: gerenciar-bde setidentifier
description: Tópico de referência para o comando Manage-bde setidentifier, que define o campo identificador da unidade na unidade para o valor especificado na configuração fornecer os identificadores exclusivos para sua organização Política de Grupo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7092d18f-4ac9-4c73-a20f-1246ca60e75e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b4a21df9d177d7bf6813abb0d418d7355d5e59a
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222605"
---
# <a name="manage-bde-setidentifier"></a>gerenciar-bde setidentifier

Define o campo identificador da unidade na unidade para o valor especificado na configuração **fornecer os identificadores exclusivos para sua organização** política de grupo.

## <a name="syntax"></a>Sintaxe

```
manage-bde –setidentifier <drive> [-computername <name>] [{-?|/?}] [{-help|-h}]
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

Para definir o campo identificador de unidade do BitLocker para C, digite:

```
manage-bde –setidentifier C:
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Manage-bde](manage-bde.md)

- [Guia de recuperação do BitLocker](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-recovery-guide-plan)
