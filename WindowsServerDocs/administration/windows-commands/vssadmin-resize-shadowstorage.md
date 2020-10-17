---
title: vssadmin resize shadowstorage
description: Uma descrição do comando vssadmin Resize ShadowStorage, que redimensiona a quantidade máxima de espaço de armazenamento que pode ser usada para armazenamento de cópia de sombra.
ms.topic: reference
author: JasonGerend
ms.author: jgerend
ms.date: 03/05/2020
ms.localizationpriority: medium
ms.openlocfilehash: 704130890d68c271e74a9163ba4ae76dcfb1a516
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156141"
---
# <a name="vssadmin-resize-shadowstorage"></a>vssadmin resize shadowstorage

> Aplica-se a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Redimensiona a quantidade máxima de espaço de armazenamento que pode ser usada para armazenamento de cópia de sombra.

A quantidade mínima de espaço de armazenamento que pode ser usada para armazenamento de cópia de sombra pode ser especificada usando o valor do registro **MinDiffAreaFileSize** . Para obter mais informações, consulte [MinDiffAreaFileSize](/windows/win32/backup/registry-keys-for-backup-and-restore#mindiffareafilesize).

> [!WARNING]
> Redimensionar a associação de armazenamento pode fazer com que as cópias de sombra desapareçam.

## <a name="syntax"></a>Sintaxe

```
vssadmin resize shadowstorage /for=<ForVolumeSpec> /on=<OnVolumeSpec> [/maxsize=<MaxSizeSpec>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| /for =`<ForVolumeSpec>` | Especifica o volume para o qual a quantidade máxima de espaço de armazenamento deve ser redimensionada. |
| /on =`<OnVolumeSpec>` | Especifica o volume de armazenamento. |
| [/MaxSize = `<MaxSizeSpec>` ] | Especifica a quantidade máxima de espaço que pode ser usada para armazenar cópias de sombra. Se nenhum valor for especificado para **/MaxSize**, não haverá limite para a quantidade de espaço de armazenamento que pode ser usada.<p>O valor de **MaxSizeSpec** deve ser 1 MB ou maior e deve ser expresso em uma das seguintes unidades: KB, MB, GB, TB, PB ou EB. Se nenhuma unidade for especificada, **MaxSizeSpec** usará bytes por padrão. |

## <a name="examples"></a>Exemplos

Para redimensionar a cópia de sombra do volume C no volume D, com um tamanho máximo de 900MB, digite:

```
vssadmin resize shadowstorage /For=C: /On=D: /MaxSize=900MB
```

Para redimensionar a cópia de sombra do volume C no volume D, sem tamanho máximo, digite:

```
vssadmin resize shadowstorage /For=C: /On=D: /MaxSize=UNBOUNDED
```

Para redimensionar a cópia de sombra do volume C em 20%, digite:

```
vssadmin resize shadowstorage /For=C: /On=C: /MaxSize=20%
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando VSSadmin](vssadmin.md)
