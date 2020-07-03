---
title: gerenciar o desbloqueio automático do bde
description: Artigo de referência para o comando autolock do Manage-bde, que gerencia o desbloqueio automático de unidades de dados protegidas pelo BitLocker.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a81d3e864a33efd5a6a1c81a5a193338d2c25bfa
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931632"
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