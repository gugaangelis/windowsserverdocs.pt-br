---
title: Carregar metadados
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2c535487-668b-44fc-babb-ff59cf7d190e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3d3132bca86533ec3f2d0a27247bd3c116cf55b6
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724461"
---
# <a name="load-metadata"></a>Carregar metadados



Carrega um arquivo Metadata. cab antes de importar uma cópia de sombra transportável ou carrega os metadados do gravador no caso de uma restauração. Se usado sem parâmetros, **carregar metadados** exibe a ajuda no prompt de comando.



## <a name="syntax"></a>Sintaxe

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Unidade>:] [<Path>]|Especifica o local do arquivo de metadados.|
|MetaData. cab|Especifica o arquivo Metadata. cab a ser carregado.|

## <a name="remarks"></a>Comentários

-   Você pode usar o comando de **importação** para importar uma cópia de sombra transportável com base nos metadados especificados pelos **metadados de carga**.
-   Esse comando é necessário antes do comando **Iniciar restauração** para carregar os gravadores e componentes selecionados para a restauração.

## <a name="examples"></a>Exemplos

Para carregar um arquivo de metadados chamado Metafile. cab do local padrão, digite:
```
load metadata metafile.cab
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)