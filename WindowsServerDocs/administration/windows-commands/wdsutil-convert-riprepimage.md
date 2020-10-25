---
title: Convert-RiprepImage
description: Artigo de referência para o comando Convert-RiprepImage, que converte uma imagem existente do RIPrep (preparação de instalação remota) no formato de imagem do Windows (. wim).
ms.topic: reference
ms.assetid: 88c2b96f-5947-4b64-9dcf-1946b3c013cf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 4062aa747bd02956a498c16c9e8aa15e96d8170d
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524921"
---
# <a name="convert-riprepimage"></a>Convert-RiprepImage

Converte uma imagem existente do RIPrep (preparação de instalação remota) no formato de imagem do Windows (. wim).

## <a name="syntax"></a>Sintaxe

```
wdsutil [Options] /Convert-RIPrepImage /FilePath:<Filepath and name> /DestinationImage /FilePath:<Filepath and name> [/Name:<Name>] [/Description:<Description>] [/InPlace] [/Overwrite:{Yes | No | Append}]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| FilePath`<Filepath and name>` | Especifica o filePath completo e o nome do arquivo. sif que corresponde à imagem RIPrep. Esse arquivo é normalmente chamado de RIPrep. sif e é encontrado na subpasta **\Templates** da pasta que contém a imagem RIPREP. |
| /DestinationImage | Especifica as configurações para a imagem de destino.  Usa as seguintes opções:<ul><li>`/FilePath:<Filepath and name>` – Define o caminho completo do arquivo para o novo arquivo. Por exemplo: **C:\Temp\convert.wim**</li><li>[ `/Name:<Name>` ] – Define o nome de exibição da imagem. Se nenhum nome de exibição for especificado, o nome de exibição da imagem de origem será usado.</li><li>[ `/Description:<Description>` ] – Define a descrição da imagem.</li><li>[/InPlace] – Especifica que a conversão deve ocorrer na imagem original do RIPrep e não em uma cópia da imagem original, que é o comportamento padrão.</li><li>[ `/Overwrite:{Yes | No | Append}` -Define se essa imagem deve substituir ou anexar arquivos existentes.</li></ul> |

## <a name="examples"></a>Exemplos

Para converter a imagem RIPrep. sif especificada em RIPREP. wim, digite:

```
wdsutil /Convert-RiPrepImage /FilePath:R:\RemoteInstall\Setup\English \Images\Win2k3.SP1\i386\Templates\riprep.sif /DestinationImage /FilePath:C:\Temp\RIPREP.wim
```

Para converter a imagem RIPrep. sif especificada em RIPREP. wim com o nome e a descrição especificados e substituí-la pelo novo arquivo se já existir um arquivo, digite:

```
wdsutil /Verbose /Progress /Convert-RiPrepImage /FilePath:\\Server \RemInst\Setup\English\Images\WinXP.SP2\i386\Templates\riprep.sif /DestinationImage /FilePath:\\Server\Share\RIPREP.wim /Name:WindowsXP image /Description:Converted RIPREP image of WindowsXP /Overwrite:Append
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Cmdlets dos serviços de implantação do Windows](/powershell/module/wds)
