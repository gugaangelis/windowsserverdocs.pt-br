---
title: move
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: d651e586c31ff64664079bdd10ffde3701ec317d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824987"
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
|/-y|Faz com que a solicitação de confirmação que você deseja substituir um arquivo de destino existente.|
|\<origem >|Especifica o caminho e nome do arquivo ou arquivos a serem movidos. Se você deseja mover ou renomear um diretório *origem* deve ser o nome e caminho do diretório atual.|
|\<Destino >|Especifica o caminho e nome para mover arquivos para. Se você deseja mover ou renomear um diretório *destino* deve ser o nome e caminho do diretório desejado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   O **/y** opção de linha de comando pode estar predefinida na variável de ambiente COPYCMD. Você pode substituí-lo com **y/** na linha de comando. O padrão é a solicitação antes de sobrescrever arquivos, a menos que o **cópia** comando é executado dentro de um script em lotes.
-   Movendo arquivos criptografados para um volume que não oferece suporte a resultados de Encrypting File System (EFS) em um erro. Descriptografar os arquivos pela primeira vez ou mover os arquivos para um volume que dá suporte a EFS.

## <a name="BKMK_examples"></a>Exemplos

Para mover todos os arquivos com a extensão. xls da pasta \Data para a pasta \Second_Q\Reports, digite:
```
move \data\*.xls \second_q\reports\ 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)