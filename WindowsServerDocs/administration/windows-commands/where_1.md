---
title: onde
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ff50405dd53ee383abc8e13f67befecf73e37c1d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832447"
---
# <a name="where"></a>onde



Exibe o local dos arquivos que correspondem ao padrão de pesquisa fornecido.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...] 
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/r \<Dir>|Indica uma pesquisa recursiva, começando com o diretório especificado.|
|/q|Retorna um código de saída (**0** para ter êxito, **1** falha) sem exibir a lista de arquivos correspondentes.|
|/f|Exibe os resultados do **onde** comando entre aspas.|
|/t|Exibe o tamanho do arquivo e a data da última modificação e a hora de cada arquivo correspondente.|
|[$\<ENV>:\|\<Path>:]\<Pattern>[ ...]|Especifica o padrão de pesquisa para os arquivos corresponder. Pelo menos um padrão é necessário, e o padrão pode incluir caracteres curinga (**&#42;** e **?**). Por padrão, **onde** pesquisará o diretório atual e os caminhos que são especificados na variável de ambiente PATH. Você pode especificar um caminho diferente para pesquisar usando o formato $*ENV*:*padrão* (onde *ENV* é uma variável de ambiente, que contém um ou mais caminhos) ou usando o o formato *caminho*:*padrão* (onde *caminho* é o caminho do diretório que você deseja pesquisar). Esses formatos opcionais não devem ser usados com o **/r** opção de linha de comando.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Se você não especificar uma extensão de nome de arquivo, as extensões listadas na variável de ambiente PATHEXT são acrescentadas ao padrão por padrão.
-   **Onde** pode executar pesquisas recursivas, exibir informações de arquivo, como a data ou o tamanho e aceite as variáveis de ambiente no lugar de caminhos em computadores locais.

## <a name="BKMK_examples"></a>Exemplos

Para localizar todos os arquivos chamados Test na unidade C do computador atual e seus subdiretórios, digite:
```
where /r c:\ test 
```
Para listar todos os arquivos no diretório público, digite:
```
where $public:*.*
```
Para localizar todos os arquivos chamados Notepad na unidade C do computador remoto, Computador1 e seus subdiretórios, digite:
```
where /r \\computer1\c notepad.*
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)