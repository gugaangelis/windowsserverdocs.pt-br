---
title: path
description: Saiba como definir a variável de ambiente PATH.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1bfa1349-e79a-472b-a9e6-d7a91149ae8f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cb77cac3871dcf4a411638409de68d038a317d24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837709"
---
# <a name="path"></a>path



Define o caminho de comando na variável de ambiente PATH (o conjunto de diretórios usado para pesquisar arquivos executáveis). Se usado sem parâmetros, **path** exibe o caminho do comando atual.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
path [[<Drive>:]<Path>[;...][;%PATH%]]
path ;
```

### <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                                                                     Descrição                                                                                                      |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<drive >:]<Path> |                                                                            Especifica a unidade e o diretório a serem definidos no caminho de comando.                                                                             |
|         ;         | Separa diretórios no caminho de comando. Se usado sem outros parâmetros, **;** limpa os caminhos de comando existentes da variável de ambiente Path e direciona o cmd. exe para pesquisar somente no diretório atual. |
|      Multi-Path       |                                                         Anexa o caminho do comando ao conjunto existente de diretórios listados na variável de ambiente PATH.                                                         |
|        /?         |                                                                                         Exibe a ajuda no prompt de comando.                                                                                         |

## <a name="remarks"></a>Comentários

-   Quando você inclui **% path%** na sintaxe, o cmd. exe o substitui pelos valores de caminho de comando encontrados na variável de ambiente Path, eliminando a necessidade de inserir esses valores manualmente no prompt de comando.
-   O diretório atual sempre é pesquisado antes dos diretórios especificados no caminho do comando.
-   Talvez você tenha arquivos em um diretório que compartilhem o mesmo nome de arquivo, mas que tenham extensões diferentes. Por exemplo, você pode ter um arquivo chamado Accnt.com que inicia um programa de contabilidade e outro arquivo chamado accnt. bat que conecta o servidor à rede de sistema de contabilidade.

    O sistema operacional Windows procura um arquivo usando extensões de nome de arquivo padrão na seguinte ordem de precedência:. exe,. com,. bat e. cmd. Para executar accnt. bat quando o Accnt.com existe no mesmo diretório, você deve incluir a extensão. bat no prompt de comando.
-   Se dois ou mais arquivos no caminho de comando tiverem o mesmo nome de arquivo e extensão, o **caminho** primeiro procurará o nome de arquivo especificado no diretório atual. Em seguida, ele pesquisa os diretórios no caminho do comando na ordem em que estão listados na variável de ambiente PATH.
-   Se você posicionar o comando **path** no arquivo autoexec. NT, o sistema operacional Windows acrescentará automaticamente o caminho de pesquisa do subsistema MS-dos especificado sempre que você fizer logon no computador. O cmd. exe não usa o arquivo autoexec. NT. Quando iniciado a partir de um atalho, o cmd. exe herda as variáveis de ambiente definidas em Meu Computador/Propriedades/avançado/ambiente.

## <a name="examples"></a><a name="BKMK_examples"></a>Disso

Para pesquisar os caminhos C:\User\Taxes, B:\User\Invest e B:\Bin para comandos externos, digite:

`path c:\user\taxes;b:\user\invest;b:\bin`

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)