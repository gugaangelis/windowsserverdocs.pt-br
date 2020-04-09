---
title: árvore
description: Tópico de comandos do Windows para árvore, que exibe a estrutura de diretório de um caminho, ou do disco em uma unidade, graficamente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 345d3192-401e-4a3b-a8ac-36a85c7be79d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14b9a4dfd5c84b55a32dbc3f6fd7e8a2cc00c7ba
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832669"
---
# <a name="tree"></a>árvore

Exibe a estrutura de diretório de um caminho ou do disco em uma unidade graficamente.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
tree [<Drive>:][<Path>] [/f] [/a]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|> da unidade de \<:|Especifica a unidade que contém o disco para o qual você deseja exibir a estrutura do diretório.|
|\<caminho >|Especifica o diretório para o qual você deseja exibir a estrutura de diretório.|
|/f|Exibe os nomes dos arquivos em cada diretório.|
|/a|Especifica que a **árvore** deve usar caracteres de texto em vez de caracteres gráficos para mostrar as linhas que vinculam subdiretórios.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

A estrutura exibida pela **árvore** depende dos parâmetros que você especificar no prompt de comando. Se você não especificar uma unidade ou caminho, a **árvore** exibirá a estrutura de árvore que começa com o diretório atual da unidade atual.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exibir os nomes de todos os subdiretórios no disco na unidade atual, digite:
```
tree \
```
Para exibir uma tela de cada vez, os arquivos em todos os diretórios na unidade C, digite:
```
tree c:\ /f | more 
```
Para imprimir uma lista de todos os diretórios na unidade C, digite:
```
tree c:\ /f  prn 
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)