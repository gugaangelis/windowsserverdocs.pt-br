---
title: move
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4302a3403f73892500c3f9deb608e9489c7e91ae
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373532"
---
# <a name="move"></a>move



Move um ou mais arquivos de um diretório para outro diretório.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
move [{/y | /-y}] [<Source>] [<Target>]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/y|Suprime a solicitação para confirmar que você deseja substituir um arquivo de destino existente.|
|/-y|Faz com que o prompt confirme que você deseja substituir um arquivo de destino existente.|
|\<Source >|Especifica o caminho e o nome do arquivo ou dos arquivos a serem movidos. Se você quiser mover ou renomear um diretório, a *origem* deverá ser o nome e o caminho do diretório atual.|
|\<Target >|Especifica o caminho e o nome para os quais mover os arquivos. Se você quiser mover ou renomear um diretório, o *destino* deverá ser o caminho e o nome do diretório desejado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   A opção de linha de comando **/y** pode ser predefinida na variável de ambiente COPYCMD. Você pode substituir isso por **/-y** na linha de comando. O padrão é solicitar antes de substituir arquivos, a menos que o comando de **cópia** seja executado de dentro de um script em lotes.
-   Mover arquivos criptografados para um volume que não dá suporte a Encrypting File System (EFS) resulta em um erro. Descriptografe os arquivos primeiro ou mova os arquivos para um volume que dê suporte ao EFS.

## <a name="BKMK_examples"></a>Disso

Para mover todos os arquivos com a extensão. xls do diretório \data para o diretório \Second_Q\Reports, digite:
```
move \data\*.xls \second_q\reports\ 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)