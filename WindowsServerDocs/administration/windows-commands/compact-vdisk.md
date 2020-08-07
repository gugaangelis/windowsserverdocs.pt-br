---
title: compact vdisk
description: Artigo de referência para o comando Compact VDISK, que reduz o tamanho físico de um arquivo de disco rígido virtual (VHD) de expansão dinâmica.
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e8f29cf7188d2630f15bee9bde2910c64f325b5
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892625"
---
# <a name="compact-vdisk"></a>compact vdisk

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reduz o tamanho físico de um arquivo de disco rígido virtual (VHD) de expansão dinâmica. Esse parâmetro é útil porque a expansão dinâmica de VHDs aumenta em tamanho à medida que você adiciona arquivos, mas eles não são reduzidos de tamanho automaticamente quando você exclui arquivos.

## <a name="syntax"></a>Sintaxe

```
compact vdisk
```

### <a name="remarks"></a>Comentários

- Um VHD de expansão dinâmica deve ser selecionado para que essa operação seja realizada com sucesso. Use o [comando SELECT VDISK](select-vdisk.md) para selecionar um VHD e deslocar o foco para ele.

- Você só pode usar VHDs de expansão dinâmica compactados que são desanexados ou anexados como somente leitura.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando attach vdisk](attach-vdisk.md)

- [comando VDISK do detalhe](detail-vdisk.md)

- [Desanexar comando vdisk](detach-vdisk.md)

- [comando expandir vdisk](expand-vdisk.md)

- [Comando Merge vdisk](merge-vdisk.md)

- [Selecionar comando vdisk](select-vdisk.md)

- [comando de lista](list.md)
