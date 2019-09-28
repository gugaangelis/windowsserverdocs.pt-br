---
title: Usando o comando Convert-RiprepImage
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b0b5e75a4148359db5088e7ff60d29b8d157309d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71363652"
---
# <a name="using-the-convert-riprepimage-command"></a>Usando o comando Convert-RiprepImage



Converte uma imagem existente do RIPrep (preparação de instalação remota) no formato de imagem do Windows (. wim).

## <a name="syntax"></a>Sintaxe

```
WDSUTIL [Options] /Convert-RIPrepImage /FilePath:<File path and name>
     /DestinationImage
         /FilePath:<File path and name>
         [/Name:<Name>]
         [/Description:<Description>]
         [/InPlace]
         [/Overwrite:{Yes | No | Append}]
```

## <a name="parameters"></a>Parâmetros

|            Parâmetro            |                                                                                                                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                                                                                                                |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /FilePath.: > nome e caminho do 0File @no__t |                                                                                                                                                                                                       Especifica o caminho completo e o nome do arquivo. sif que corresponde à imagem RIPrep. Esse arquivo é normalmente chamado de RIPrep. sif e é encontrado na subpasta \Templates da pasta que contém a imagem RIPrep.                                                                                                                                                                                                       |
|        /DestinationImage        | Especifica as configurações para a imagem de destino, usando as opções a seguir.</br>-/FilePath.: o caminho e o > nome do 0File de @no__t – define o caminho completo do arquivo para o novo arquivo. Por exemplo: **C:\Temp\convert.wim**</br>-[/Name: \<Name >] – define o nome de exibição da imagem. Se nenhum nome de exibição for especificado, o nome de exibição da imagem de origem será usado.</br>- [/Description: \<Description >] – define a descrição da imagem.</br>-[/InPlace]-Especifica que a conversão deve ocorrer na imagem original do RIPrep e não em uma cópia da imagem original, que é o comportamento padrão.</br>-[/Overwrite: {Sim |

## <a name="BKMK_examples"></a>Disso

Para converter a imagem RIPrep. sif especificada em RIPREP. wim, digite:
```
WDSUTIL /Convert-RiPrepImage /FilePath:"R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif" /DestinationImage
/FilePath:"C:\Temp\RIPREP.wim"
```
Para converter a imagem RIPrep. sif especificada em RIPREP. wim com o nome e a descrição especificados e substituí-la pelo novo arquivo se já existir um arquivo, digite:
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:"\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif"
/DestinationImage /FilePath:"\\Server\Share\RIPREP.wim"
/Name:"WindowsXP image" /Description:"Converted RIPREP image of WindowsXP"
/Overwrite:Append
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)