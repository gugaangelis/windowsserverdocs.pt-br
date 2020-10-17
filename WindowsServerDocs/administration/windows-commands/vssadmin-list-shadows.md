---
title: vssadmin list shadows
description: Uma descrição do comando VSSadmin list Shadows, que lista todas as cópias de sombra existentes de um volume especificado.
ms.topic: reference
author: JasonGerend
ms.author: jgerend
ms.date: 05/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: cb7ddaa09e2da350c1c5422c6224aabd8831f763
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156205"
---
# <a name="vssadmin-list-shadows"></a>vssadmin list shadows

> Aplica-se a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Lista todas as cópias de sombra existentes de um volume especificado. Se você usar esse comando sem parâmetros, ele exibirá todas as cópias de sombra de volume no computador na ordem determinada pelo **conjunto de cópias de sombra**.

## <a name="syntax"></a>Sintaxe

```
vssadmin list shadows [/for=<ForVolumeSpec>] [/shadow=<ShadowID>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /for =`<ForVolumeSpec>` | Especifica em qual volume as cópias de sombra serão listadas. |
| /Shadow =`<ShadowID>` | Lista a cópia de sombra especificada por Shadowid. Para obter a ID da cópia de sombra, use o [comando VSSadmin list Shadows](vssadmin-list-shadows.md). Ao digitar uma ID de cópia de sombra, use o seguinte formato, em que cada *X* representa um caractere hexadecimal:<p>XXXXXXXX-XXXX-XXXX-XXXX-XXXXXXXXXXXX |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando VSSadmin](vssadmin.md)

- [comando VSSadmin list Shadows](vssadmin-list-shadows.md)
