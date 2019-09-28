---
title: Carregar metadados
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 025f75743d61889c4b987e9a2a575d1c599f04c1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71374624"
---
# <a name="load-metadata"></a>Carregar metadados



Carrega um arquivo Metadata. cab antes de importar uma cópia de sombra transportável ou carrega os metadados do gravador no caso de uma restauração. Se usado sem parâmetros, **carregar metadados** exibe a ajuda no prompt de comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
load metadata [<Drive>:][<Path>]<MetaData.cab>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|[\<Drive >:] [<Path>]|Especifica o local do arquivo de metadados.|
|MetaData. cab|Especifica o arquivo Metadata. cab a ser carregado.|

## <a name="remarks"></a>Comentários

-   Você pode usar o comando de **importação** para importar uma cópia de sombra transportável com base nos metadados especificados pelos **metadados de carga**.
-   Esse comando é necessário antes do comando **Iniciar restauração** para carregar os gravadores e componentes selecionados para a restauração.

## <a name="BKMK_examples"></a>Disso

Para carregar um arquivo de metadados chamado Metafile. cab do local padrão, digite:
```
load metadata metafile.cab
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)