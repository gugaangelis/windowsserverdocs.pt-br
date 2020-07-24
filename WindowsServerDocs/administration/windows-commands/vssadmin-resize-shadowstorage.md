---
title: Vssadmin redimensionar ShadowStorage
description: Uma descrição do comando vssadmin Resize ShadowStorage.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 03/05/2020
ms.localizationpriority: medium
ms.openlocfilehash: 8b723fd3768561da7d636dd1724bd0c75ee2ee85
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954698"
---
# <a name="vssadmin-resize-shadowstorage"></a>Vssadmin redimensionar ShadowStorage

> Aplica-se a: Windows 10, Windows 8.1, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Redimensiona a quantidade máxima de espaço de armazenamento que pode ser usada para armazenamento de cópia de sombra.

A quantidade mínima de espaço de armazenamento que pode ser usada para armazenamento de cópia de sombra pode ser especificada usando o valor do registro **MinDiffAreaFileSize** . Para obter mais informações, consulte [MinDiffAreaFileSize](/windows/win32/backup/registry-keys-for-backup-and-restore#mindiffareafilesize).

> [!WARNING]
> Redimensionar a associação de armazenamento pode fazer com que as cópias de sombra desapareçam.

## <a name="syntax"></a>Sintaxe

```cmd
vssadmin resize shadowstorage /for=<ForVolumeSpec> /on=<OnVolumeSpec> [/maxsize=<MaxSizeSpec>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---|---|
`/for=<ForVolumeSpec>`  | Especifica o volume para o qual a quantidade máxima de espaço de armazenamento deve ser redimensionada.
`/on=<OnVolumeSpec>` | Especifica o volume de armazenamento.
`[/maxsize=<MaxSizeSpec>]` |  Especifica a quantidade máxima de espaço que pode ser usada para armazenar cópias de sombra. Se nenhum valor for especificado para/MaxSize, não haverá nenhum limite para a quantidade de espaço de armazenamento que pode ser usada.  <br> <br> O valor de MaxSizeSpec deve ser 1 MB ou maior e deve ser expresso em uma das seguintes unidades: KB, MB, GB, TB, PB ou EB. Se nenhuma unidade for especificada, MaxSizeSpec usará bytes por padrão.

## <a name="examples"></a>Exemplos

```cmd
vssadmin Resize ShadowStorage /For=C: /On=D: /MaxSize=900MB
vssadmin Resize ShadowStorage /For=C: /On=D: /MaxSize=UNBOUNDED
vssadmin Resize ShadowStorage /For=C: /On=C: /MaxSize=20%
```

## <a name="additional-references"></a>Referências adicionais

* [Chave de sintaxe de linha de comando](./command-line-syntax-key.md)
* [Vssadmin](vssadmin.md)
