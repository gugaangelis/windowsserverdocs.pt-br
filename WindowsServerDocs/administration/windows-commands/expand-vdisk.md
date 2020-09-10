---
title: expand vdisk
description: Artigo de referência para o comando Expand VDISK, que expande um disco rígido virtual (VHD) para um tamanho especificado.
ms.topic: reference
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0d3a959dceee45f1ef9f6fda73dc283d57ddef1c
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635947"
---
# <a name="expand-vdisk"></a>expand vdisk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Expande um disco rígido virtual (VHD) para um tamanho especificado.

Um VHD deve ser selecionado e desanexado para que essa operação tenha sucesso. Use o [comando SELECT VDISK](select-vdisk.md) para selecionar um volume e deslocar o foco para ele.

## <a name="syntax"></a>Sintaxe

```
expand vdisk maximum=<n>
```

### <a name="parameters"></a>Parâmetros

 | Parâmetro | Descrição |
 |---------- | ----------- |
 | máximo =`<n>` | Especifica o novo tamanho para o VHD em megabytes (MB). |

### <a name="examples"></a>Exemplos

Para expandir o VHD selecionado para 20 GB, digite:

```
expand vdisk maximum=20000
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Selecionar comando vdisk](select-vdisk.md)

- [comando attach vdisk](attach-vdisk.md)

- [comando Compact vdisk](compact-vdisk.md)

- [desanexar comando vdisk](detach-vdisk.md)

- [comando VDISK do detalhe](detail-vdisk.md)

- [comando Merge vdisk](merge-vdisk.md)

- [comando de lista](list.md)
