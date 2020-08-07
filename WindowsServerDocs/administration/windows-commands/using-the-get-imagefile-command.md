---
title: obter o ImageFile
description: Artigo de referência para Get-ImageFile, que recupera informações sobre as imagens contidas em um arquivo de imagem do Windows (. wim).
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 10b86929366cc8734a5ff155200fb3078f0cd5d4
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87879374"
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