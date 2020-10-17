---
title: vssadmin delete shadows
description: Uma descrição do comando VSSadmin Delete Shadows, que exclui as cópias de sombra de um volume especificado.
ms.topic: reference
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2dcd1bf8cbe946c77d0b5a100d2a29ecb36ef50f
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156222"
---
# <a name="vssadmin-delete-shadows"></a>vssadmin delete shadows

> Aplica-se a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Exclui as cópias de sombra de um volume especificado. Você só pode excluir cópias de sombra com o tipo *acessível pelo cliente* .

## <a name="syntax"></a>Sintaxe

```
vssadmin delete shadows /for=<ForVolumeSpec> [/oldest | /all | /shadow=<ShadowID>] [/quiet]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /for =`<ForVolumeSpec>` | Especifica qual cópia de sombra do volume será excluída. |
| /oldest | Exclui somente a cópia de sombra mais antiga. |
| /all | Exclui todas as cópias de sombra do volume especificado. |
| /Shadow =`<ShadowID>` | Exclui a cópia de sombra especificada por Shadowid. Para obter a ID da cópia de sombra, use o [comando VSSadmin list Shadows](vssadmin-list-shadows.md). Ao inserir uma ID de cópia de sombra, use o seguinte formato, em que cada *X* representa um caractere hexadecimal:<p>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX |
| /quiet | Especifica que o comando não exibirá mensagens durante a execução. |

## <a name="examples"></a>Exemplos

Para excluir a cópia de sombra mais antiga do volume C, digite:

```
vssadmin delete shadows /for=c: /oldest
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando VSSadmin](vssadmin.md)

- [comando VSSadmin list Shadows](vssadmin-list-shadows.md)
