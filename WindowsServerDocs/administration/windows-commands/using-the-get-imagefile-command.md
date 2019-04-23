---
title: Usando o comando get-ImageFile
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e1e296fb-20cf-4a60-9db4-4cbac7d4dab5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bbe5ece95d1f9821a27b96e56bc34576a0f5f33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827617"
---
# <a name="using-the-get-imagefile-command"></a>Usando o comando get-ImageFile



Recupera informações sobre as imagens contidos em um arquivo de imagem do Windows (. wim).

## <a name="syntax"></a>Sintaxe

```
WDSUTIL [Options] /Get-ImageFile /ImageFile:<wim file path> [/Detailed]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/ ImageFile:\<caminho do arquivo WIM >|Especifica o nome de arquivo e caminho completo do arquivo. wim.|
|[/Detailed]|Retorna todos os metadados de imagem de cada imagem. Se essa opção não for usada, o comportamento padrão é retornar somente o nome da imagem, descrição e nome de arquivo.|

## <a name="BKMK_examples"></a>Exemplos

Para exibir informações sobre uma imagem, digite:
```
WDSUTIL /Get-ImageFile /ImageFile:"C:\temp\install.wim"
```
Para exibir informações detalhadas, digite:
```
WDSUTIL /Verbose /Get-ImageFile /ImageFile:"\\Server\Share\My Folder \install.wim" /Detailed
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)