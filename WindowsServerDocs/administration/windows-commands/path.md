---
title: caminho
description: Tópico de referência para definir o caminho de comando na variável de ambiente PATH, especificando o conjunto de diretórios usado para pesquisar arquivos executáveis (. exe).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6fadeb2108f0e59ee2f45f3cf45338046a345006
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472562"
---
# <a name="path"></a>caminho

Define o caminho de comando na variável de ambiente PATH, especificando o conjunto de diretórios usados para pesquisar arquivos executáveis (. exe). Se usado sem parâmetros, este comando exibe o caminho do comando atual.

## <a name="syntax"></a>Sintaxe

```
path [[<drive>:]<path>[;...][;%PATH%]]
path ;
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `[<drive>:]<path>` | Especifica a unidade e o diretório a serem definidos no caminho de comando. O diretório atual sempre é pesquisado antes dos diretórios especificados no caminho do comando. |
| ; | Separa diretórios no caminho de comando. Se usado sem outros parâmetros, **;** limpa os caminhos de comando existentes da variável de ambiente Path e direciona Cmd.exe para pesquisar somente no diretório atual. |
| `%PATH%` | Anexa o caminho do comando ao conjunto existente de diretórios listados na variável de ambiente PATH. Se você incluir esse parâmetro, Cmd.exe o substituirá pelos valores de caminho de comando encontrados na variável de ambiente PATH, eliminando a necessidade de inserir esses valores manualmente no prompt de comando. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="remarks"></a>Comentários


- O sistema operacional Windows pesquisa usando extensões de nome de arquivo padrão na seguinte ordem de precedência:. exe,. com,. bat e. cmd. Ou seja, se você estiver procurando um arquivo em lotes chamado, acct.bat, mas tiver um aplicativo chamado acct.exe no mesmo diretório, deverá incluir a extensão. bat no prompt de comando.

- Se dois ou mais arquivos no caminho de comando tiverem o mesmo nome de arquivo e extensão, esse comando procurará primeiro o nome de arquivo especificado no diretório atual. Em seguida, ele pesquisa os diretórios no caminho do comando na ordem em que estão listados na variável de ambiente PATH.

- Se você posicionar o comando **path** no arquivo autoexec. NT, o sistema operacional Windows acrescentará automaticamente o caminho de pesquisa do subsistema MS-dos especificado sempre que você fizer logon no computador. Cmd.exe não usa o arquivo autoexec. NT. Quando iniciado a partir de um atalho, Cmd.exe herda as variáveis de ambiente definidas em Meu Computador/Propriedades/avançado/ambiente.

## <a name="examples"></a>Exemplos

Para pesquisar os caminhos *c:\user\taxes*, *b:\user\invest*e *b:\bin* para comandos externos, digite:

```
path c:\user\taxes;b:\user\invest;b:\bin
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
