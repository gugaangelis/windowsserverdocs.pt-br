---
title: Carregar metadados
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b52b5040fc8c834b04cad83ca4b0cfab103fdc43
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59871327"
---
# <a name="load-metadata"></a>Carregar metadados



Carrega um arquivo. cab de metadados antes de importar uma cópia de sombra transportáveis ou carrega os metadados do gravador no caso de uma restauração. Se usado sem parâmetros, **carregar metadados** exibe a Ajuda no prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive>:][<Path>]|Especifica o local do arquivo de metadados.|
|MetaData.cab|Especifica o arquivo. cab de metadados para carregar.|

## <a name="remarks"></a>Comentários

-   Você pode usar o **importação** comando para importar uma cópia de sombra transportáveis baseado nos metadados especificado por **carregar metadados**.
-   Esse comando é necessária antes do **Iniciar restauração** comando para carregar os gravadores selecionados e os componentes para a restauração.

## <a name="BKMK_examples"></a>Exemplos

Para carregar um arquivo de metadados chamado metafile.cab do local padrão, digite:
```
load metadata metafile.cab
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)