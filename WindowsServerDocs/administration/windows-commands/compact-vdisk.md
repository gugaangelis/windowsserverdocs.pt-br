---
title: Compact vdisk
description: Os comandos do Windows tópico forcompact VDISK, que reduz o tamanho físico de um arquivo de disco rígido virtual (VHD) de expansão dinâmica.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9691be21c188fbc2c3b2e782acde127270decf56
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847419"
---
# <a name="compact-vdisk"></a>Compact vdisk

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reduz o tamanho físico de um arquivo de disco rígido virtual (VHD) de expansão dinâmica. Esse parâmetro é útil porque a expansão dinâmica de VHDs aumenta em tamanho à medida que você adiciona arquivos, mas eles não são reduzidos de tamanho automaticamente quando você exclui arquivos.

> [!NOTE]
> Esse comando só é aplicável ao Windows 7 e ao Windows Server 2008 R2.

## <a name="syntax"></a>Sintaxe
```
compact vdisk
```

## <a name="remarks"></a>Comentários

- Um VHD de expansão dinâmica deve ser selecionado para que essa operação seja realizada com sucesso. Use o comando **Select VDISK** para selecionar um VHD e deslocar o foco para ele.

- Você só pode compactar VHDs de expansão dinâmica que são desanexados ou anexados como somente leitura.

## <a name="examples"></a><a name=BKMK_Examples></a>Disso
Para compactar um VHD de expansão dinâmica, digite:
```
compact vdisk
```

## <a name="additional-references"></a>Referências adicionais
- - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [anexar vdisk](attach-vdisk.md)
- [detalhes do VDISK](detail-vdisk.md)
- [Desanexar vdisk](detach-vdisk.md)
- [expandir vdisk](expand-vdisk.md)
- [Mesclar vdisk](merge-vdisk.md)
- [selecionar vdisk](select-vdisk.md)
- [list_1](list_1.md)
