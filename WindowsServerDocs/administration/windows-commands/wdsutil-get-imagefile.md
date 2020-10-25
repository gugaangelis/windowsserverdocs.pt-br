---
title: obter o ImageFile
description: Artigo de referência para Get-ImageFile, que recupera informações sobre as imagens contidas em um arquivo de imagem do Windows (. wim).
ms.topic: reference
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 41183f92d75736afc32dfbbee9d31871b3ef24f9
ms.sourcegitcommit: 554d274fea48a4d47c19845d969a9ec93dec82de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/24/2020
ms.locfileid: "92524331"
---
# <a name="get-imagefile"></a>obter o ImageFile

Recupera informações sobre as imagens contidas em um arquivo de imagem do Windows (. wim).

## <a name="syntax"></a>Sintaxe

```
wdsutil [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/ImageFile:\<WIM file path>|Especifica o caminho completo e o nome do arquivo. wim.|
|[/Detailed]|Retorna todos os metadados de imagem de cada imagem. Se essa opção não for usada, o comportamento padrão será retornar apenas o nome da imagem, a descrição e o nome do arquivo.|

## <a name="examples"></a>Exemplos

Para exibir informações sobre uma imagem, digite:
```
wdsutil /Get-ImageFile /ImageFile:C:\temp\install.wim
```
Para exibir informações detalhadas, digite:
```
wdsutil /Verbose /Get-ImageFile /ImageFile:\\Server\Share\My Folder \install.wim /Detailed
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)