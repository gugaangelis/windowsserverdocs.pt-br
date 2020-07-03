---
title: expand vdisk
description: Artigo de referência para o comando Expand VDISK, que expande um disco rígido virtual (VHD) para um tamanho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3ae547b4-3813-4b86-bacd-bc273c028a2a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a7951d4875249e46d850883f7863262774dd9bab
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85932300"
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
