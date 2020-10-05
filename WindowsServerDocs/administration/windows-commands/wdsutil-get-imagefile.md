---
title: obter o ImageFile
description: Artigo de referência para Get-ImageFile, que recupera informações sobre as imagens contidas em um arquivo de imagem do Windows (. wim).
ms.topic: reference
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: c358c490424406e6eb8d709c879f043ce017164d
ms.sourcegitcommit: 720455aad2bac78cf64997d196a13f35ea0acb73
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2020
ms.locfileid: "91729639"
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
|/ImageFile:\<WIM file path>|Especifica o caminho completo e o nome do arquivo. wim.|
|[/Detailed]|Retorna todos os metadados de imagem de cada imagem. Se essa opção não for usada, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo.|

## <a name="examples"></a>Exemplos

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