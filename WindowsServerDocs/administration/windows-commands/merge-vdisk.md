---
title: merge vdisk
description: Artigo de referência para o comando Merge VDISK, que mescla um VHD (disco rígido virtual) diferencial com seu VHD pai correspondente.
ms.topic: reference
ms.assetid: 5865bb08-89a3-406c-8328-0ef8868d03e8
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 99a51b422c0e4bb19be4227b43fba6a5ef430628
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89635162"
---
# <a name="merge-vdisk"></a>merge vdisk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Mescla um VHD (disco rígido virtual) diferencial com seu VHD pai correspondente. O VHD pai será modificado para incluir as modificações do VHD diferencial. Esse comando modifica o VHD pai. Como resultado, outros VHDs diferenciais que dependem do pai não serão mais válidos.

> [!IMPORTANT]
> Você deve escolher e desanexar um VHD para que essa operação tenha sucesso. Use o comando **Select VDISK** para selecionar um VHD e deslocar o foco para ele.

## <a name="syntax"></a>Sintaxe

```
merge vdisk depth=<n>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| profundidade =`<n>` | Indica o número de arquivos VHD pai a serem mesclados. Por exemplo, `depth=1` indica que o VHD diferencial será mesclado com um nível da cadeia diferencial. |

### <a name="examples"></a>Exemplos

Para mesclar um VHD diferencial com seu VHD pai, digite:

```
merge vdisk depth=1
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando attach vdisk](attach-vdisk.md)

- [comando Compact vdisk](compact-vdisk.md)

- [comando VDISK do detalhe](detail-vdisk.md)

- [desanexar comando vdisk](detach-vdisk.md)

- [comando expandir vdisk](expand-vdisk.md)

- [Selecionar comando vdisk](select-vdisk.md)

- [comando de lista](list.md)
