---
title: Convert-RiprepImage
description: Tópico de referência para Convert-RiprepImage, que converte uma imagem existente do RIPrep (preparação de instalação remota) no formato de imagem do Windows (. wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 12bdd6c49b5fdec0c0e4980a1abf7e21cefc538e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721031"
---
# <a name="convert-riprepimage"></a>Convert-RiprepImage

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

### <a name="parameters"></a>Parâmetros

|            Parâmetro            |                                                                                                                                                                                                                                                                                                               Descrição                                                                                                                                                                                                                                                                                                                |
|---------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| /FilePath.:\<caminho e nome do arquivo> |                                                                                                                                                                                                       Especifica o caminho completo e o nome do arquivo. sif que corresponde à imagem RIPrep. Esse arquivo é normalmente chamado de RIPrep. sif e é encontrado na subpasta \Templates da pasta que contém a imagem RIPrep.                                                                                                                                                                                                       |
|        /DestinationImage        | Especifica as configurações para a imagem de destino, usando as opções a seguir.</br>-/FilePath.:\<caminho e nome do arquivo>-define o caminho completo do arquivo para o novo arquivo. Por exemplo: **C:\Temp\convert.wim**</br>-[/Name:\<Name>] – define o nome de exibição da imagem. Se nenhum nome de exibição for especificado, o nome de exibição da imagem de origem será usado.</br>-[/Description: \<Description>] – define a descrição da imagem.</br>-[/InPlace]-Especifica que a conversão deve ocorrer na imagem original do RIPrep e não em uma cópia da imagem original, que é o comportamento padrão.</br>-[/Overwrite: {Sim |

## <a name="examples"></a>Exemplos

Para converter a imagem RIPrep. sif especificada em RIPREP. wim, digite:
```
WDSUTIL /Convert-RiPrepImage /FilePath:R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif /DestinationImage
/FilePath:C:\Temp\RIPREP.wim
```
Para converter a imagem RIPrep. sif especificada em RIPREP. wim com o nome e a descrição especificados e substituí-la pelo novo arquivo se já existir um arquivo, digite:
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif
/DestinationImage /FilePath:\\Server\Share\RIPREP.wim
/Name:WindowsXP image /Description:Converted RIPREP image of WindowsXP
/Overwrite:Append
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)