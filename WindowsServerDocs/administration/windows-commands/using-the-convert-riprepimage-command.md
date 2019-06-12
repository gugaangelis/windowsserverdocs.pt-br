---
title: Usando o comando convert RiprepImage
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 9b41b6dcc52c3e6700d1d18c61eceea8b990ecdf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440585"
---
# <a name="using-the-convert-riprepimage-command"></a>Usando o comando convert RiprepImage



Converte uma imagem de preparação de instalação remota (RIPrep) existente em formato de imagem do Windows (. wim).

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
| / FilePath:\<caminho e nome do arquivo > |                                                                                                                                                                                                       Especifica o nome de arquivo e caminho completo do arquivo. sif que corresponde à imagem RIPrep. Esse arquivo é chamado geralmente RIPrep e for encontrado na subpasta da pasta que contém a imagem RIPrep \Templates.                                                                                                                                                                                                       |
|        /DestinationImage        | Especifica as configurações para a imagem de destino, usando as opções a seguir.</br>-/FilePath:\<caminho e nome do arquivo >-define o caminho completo para o novo arquivo. Por exemplo:  **C:\Temp\convert.wim**</br>-[/Name:\<nome >]-define o nome de exibição da imagem. Se nenhum nome de exibição for especificada, será usado o nome de exibição da imagem de origem.</br>-[/ Descrição: \<Descrição >]-define a descrição da imagem.</br>-[/InPlace] - Especifica que a conversão deve ocorrer em imagem RIPrep original e não em uma cópia da imagem original, que é o comportamento padrão.</br>-[/Overwrite: {Sim |

## <a name="BKMK_examples"></a>Exemplos

Para converter a imagem RIPrep especificada em RIPREP.wim, digite:
```
WDSUTIL /Convert-RiPrepImage /FilePath:"R:\RemoteInstall\Setup\English
\Images\Win2k3.SP1\i386\Templates\riprep.sif" /DestinationImage
/FilePath:"C:\Temp\RIPREP.wim"
```
Para converter a imagem RIPrep especificada em RIPREP.wim com o nome especificado e a descrição e substituí-lo com o novo arquivo se já existe um arquivo, digite:
```
WDSUTIL /Verbose /Progress /Convert-RiPrepImage /FilePath:"\\Server
\RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif"
/DestinationImage /FilePath:"\\Server\Share\RIPREP.wim"
/Name:"WindowsXP image" /Description:"Converted RIPREP image of WindowsXP"
/Overwrite:Append
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)