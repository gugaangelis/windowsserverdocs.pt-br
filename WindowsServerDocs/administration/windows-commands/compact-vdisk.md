---
title: Compactar vdisk
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 40ca0820-67de-4160-b62a-e9bf63fe2790
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ba04bdc8c469ef80853b6092b8745defa1f3f19e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877297"
---
# <a name="compact-vdisk"></a>Compactar vdisk

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Reduz o tamanho físico de um arquivo de disco rígido virtual (VHD) de expansão dinâmica. Esse parâmetro é útil porque VHDs aumento no tamanho de expansão dinâmica, como adicionar arquivos, mas eles não é reduzido automaticamente quando você excluir arquivos.
> [!NOTE]
> Este comando só é aplicável ao Windows 7 e Windows Server 2008 R2.
## <a name="syntax"></a>Sintaxe
```
compact vdisk
```
## <a name="remarks"></a>Comentários
-   Um VHD de expansão dinâmica deve ser selecionado para essa operação seja bem-sucedida. Use o **selecionar o vdisk** comando para selecionar um VHD e mudar o foco a ele.
-   Você só pode compactar a expandir dinamicamente os VHDs que são desanexados ou anexados como somente leitura.
## <a name="BKMK_Examples"></a>Exemplos
Para compactar um VHD de expansão dinâmica, digite:
```
compact vdisk
```
## <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [attach vdisk](attach-vdisk.md)

-   [Detalhar vdisk](detail-vdisk.md)
-   [Desanexar vdisk](detach-vdisk.md)
-   [Expandir vdisk](expand-vdisk.md)
-   [Mesclar vdisk](merge-vdisk.md)
-   [Selecionar o vdisk](select-vdisk.md)
-   [list_1](list_1.md)
