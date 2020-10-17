---
title: where
description: Artigo de referência para Where, que exibe o local dos arquivos que correspondem ao padrão de pesquisa fornecido.
ms.topic: reference
ms.assetid: 0b3486a5-896b-4d92-84b8-e463a0b76487
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 683f467e3f023e7f859bb7543e1a3feef17ce294
ms.sourcegitcommit: f45640cf4fda621b71593c63517cfdb983d1dc6a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2020
ms.locfileid: "92156637"
---
# <a name="where"></a>where



Exibe o local dos arquivos que correspondem ao padrão de pesquisa fornecido.



## <a name="syntax"></a>Sintaxe

```
where [/r <Dir>] [/q] [/f] [/t] [$<ENV>:|<Path>:]<Pattern>[ ...]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/r \<Dir>|Indica uma pesquisa recursiva, começando com o diretório especificado.|
|/q|Retorna um código de saída (**0** para êxito, **1** para falha) sem exibir a lista de arquivos correspondentes.|
|/f|Exibe os resultados do comando **Where** entre aspas.|
|/t|Exibe o tamanho do arquivo e a data e hora da última modificação de cada arquivo correspondente.|
|[$\<ENV>:\|\<Path>:]\<Pattern>[ ...]|Especifica o padrão de pesquisa para os arquivos a serem correspondentes. Pelo menos um padrão é necessário e o padrão pode incluir caracteres curinga (**&#42;** e **?**). Por padrão, **onde** o pesquisa o diretório atual e os caminhos especificados na variável de ambiente Path. Você pode especificar um caminho diferente para pesquisar usando o formato $*env*:*Pattern* (em que *env* é uma variável de ambiente existente que contém um ou mais caminhos) ou usando o formato *caminho*:*padrão* (em que *caminho* é o caminho do diretório que você deseja pesquisar). Esses formatos opcionais não devem ser usados com a opção de linha de comando **/r** .|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Se você não especificar uma extensão de nome de arquivo, as extensões listadas na variável de ambiente PATHEXT serão acrescentadas ao padrão por padrão.
-   **Onde** o pode executar pesquisas recursivas, exibir informações do arquivo, como data ou tamanho, e aceitar variáveis de ambiente no lugar de caminhos em computadores locais.

## <a name="examples"></a>Exemplos

Para localizar todos os arquivos chamados Test na unidade C do computador atual e seus subdiretórios, digite:
```
where /r c:\ test
```
Para listar todos os arquivos no diretório público, digite:
```
where $public:*.*
```
Para localizar todos os arquivos chamados notepad na unidade C do computador remoto, Computer1 e seus subdiretórios, digite:
```
where /r \\computer1\c notepad.*
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)