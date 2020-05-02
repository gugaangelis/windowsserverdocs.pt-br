---
title: Compact vdisk
description: Tópico de referência para o comando Compact VDISK, que reduz o tamanho físico de um arquivo de disco rígido virtual (VHD) de expansão dinâmica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c4ae5c653645c9f6f3ef97501a59932682c24be3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82711145"
---
# <a name="compact-vdisk"></a>Compact vdisk

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
