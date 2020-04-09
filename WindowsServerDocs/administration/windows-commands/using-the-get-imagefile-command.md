---
title: obter o ImageFile
description: O tópico de comandos do Windows para Get-ImageFile, que recupera informações sobre as imagens contidas em um arquivo de imagem do Windows (. wim).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ef1cf2b9eec6739690d286c32d26dd84b07e348c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80830989"
---
# <a name="get-imagefile"></a>obter o ImageFile

Recupera informações sobre as imagens contidas em um arquivo de imagem do Windows (. wim).

## <a name="syntax"></a>Sintaxe

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/ImageFile: caminho de arquivo\<WIM >|Especifica o caminho completo e o nome do arquivo. wim.|
|[/Detailed]|Retorna todos os metadados de imagem de cada imagem. Se essa opção não for usada, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo.|

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exibir informações sobre uma imagem, digite:
```
WDSUTIL /Get-ImageFile /ImageFile:C:\temp\install.wim
```
Para exibir informações detalhadas, digite:
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:\\Server\Share\My Folder \install.wim /Detailed
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)