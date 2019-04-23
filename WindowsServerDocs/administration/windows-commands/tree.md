---
title: tree
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: de22de1c9d62ba79c1aa68248109cca88009703a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872627"
---
# <a name="tree"></a>tree



Exibe a estrutura do diretório de um caminho ou do disco em uma unidade graficamente.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
tree [<Drive>:][<Path>] [/f] [/a]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Drive>:|Especifica a unidade que contém o disco para o qual você deseja exibir a estrutura do diretório.|
|\<Caminho >|Especifica o diretório para o qual você deseja exibir a estrutura do diretório.|
|/f|Exibe os nomes dos arquivos em cada diretório.|
|/a|Especifica que **árvore** é usar caracteres de texto em vez de caracteres de gráfico para mostrar as linhas que vinculam os subdiretórios.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

A estrutura exibida pelo **árvore** depende dos parâmetros que você especifica no prompt de comando. Se você não especificar uma unidade ou caminho, **árvore** exibirá a estrutura de árvore iniciando com o diretório atual da unidade atual.

## <a name="BKMK_examples"></a>Exemplos

Para exibir os nomes de todas as subpastas no disco em sua unidade atual, digite:
```
tree \
```
Para exibir uma tela por vez, os arquivos em todos os diretórios na unidade C, digite:
```
tree c:\ /f | more 
```
Para imprimir uma lista de todos os diretórios na unidade C, digite:
```
tree c:\ /f  prn 
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)