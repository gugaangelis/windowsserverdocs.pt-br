---
title: move
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: efd0cd0716c564a9570647820056ab9c38e41274
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80839369"
---
# <a name="move"></a>move



Move um ou mais arquivos de um diretório para outro diretório.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
move [{/y | /-y}] [<Source>] [<Target>]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/y|Suprime a solicitação para confirmar que você deseja substituir um arquivo de destino existente.|
|/-y|Faz com que o prompt confirme que você deseja substituir um arquivo de destino existente.|
|> de origem do \<|Especifica o caminho e o nome do arquivo ou dos arquivos a serem movidos. Se você quiser mover ou renomear um diretório, a *origem* deverá ser o nome e o caminho do diretório atual.|
|\<de destino >|Especifica o caminho e o nome para os quais mover os arquivos. Se você quiser mover ou renomear um diretório, o *destino* deverá ser o caminho e o nome do diretório desejado.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   A opção de linha de comando **/y** pode ser predefinida na variável de ambiente COPYCMD. Você pode substituir isso por **/-y** na linha de comando. O padrão é solicitar antes de substituir arquivos, a menos que o comando de **cópia** seja executado de dentro de um script em lotes.
-   Mover arquivos criptografados para um volume que não dá suporte a Encrypting File System (EFS) resulta em um erro. Descriptografe os arquivos primeiro ou mova os arquivos para um volume que dê suporte ao EFS.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para mover todos os arquivos com a extensão. xls do diretório \data para o diretório \ Second_Q \Reports, digite:
```
move \data\*.xls \second_q\reports\ 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)