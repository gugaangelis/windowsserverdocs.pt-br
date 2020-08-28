---
title: localizar
description: Artigo de referência para o comando find, que procura uma cadeia de caracteres de texto em arquivos, exibindo a cadeia de caracteres de texto especificada no arquivo.
ms.topic: reference
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 34eb1f1cf3071147878f421307a91de921678cfc
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89036527"
---
# <a name="find"></a>localizar

Pesquisa uma cadeia de caracteres de texto em um ou mais arquivos e exibe linhas de texto que contêm a cadeia de caracteres especificada.

## <a name="syntax"></a>Sintaxe

```
find [/v] [/c] [/n] [/i] [/off[line]] <string> [[<drive>:][<path>]<filename>[...]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /v | Exibe todas as linhas que não contêm o especificado `<string>` . |
| /c | Conta as linhas que contêm o especificado `<string>` e exibe o total. |
| /n | Precede cada linha com o número de linha do arquivo. |
| /i | Especifica que a pesquisa não diferencia maiúsculas de minúsculas. |
| [/off [line]] | Não ignora arquivos que têm o atributo offline definido. |
| `<string>` | Obrigatórios. Especifica o grupo de caracteres (entre aspas) que você deseja pesquisar. |
| `[<drive>:][<path>]<filename>` | Especifica o local e o nome do arquivo no qual Pesquisar a cadeia de caracteres especificada. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se você não usar **/i**, esse comando pesquisará exatamente o que você especificar para a *cadeia de caracteres*. Por exemplo, esse comando trata os caracteres `a` e de `A` forma diferente. No entanto, se você usar **/i**, a pesquisa se tornará não diferencia maiúsculas de minúsculas e tratará `a` e `A` como o mesmo caractere.

- Se a cadeia de caracteres que você deseja pesquisar contiver aspas, você deverá usar aspas duplas para cada aspa contida na cadeia de caracteres (por exemplo, "" esta cadeia de caracteres contém aspas "").

- Se você omitir um nome de arquivo, esse comando agirá como um filtro, tomando entrada da fonte de entrada padrão (geralmente o teclado, um pipe (|) ou um arquivo redirecionado) e, em seguida, exibirá as linhas que contêm a *cadeia de caracteres*.

- Você pode digitar parâmetros e opções de linha de comando para o comando **Find** em qualquer ordem.

- Você não pode usar curingas (**&#42;** e **?**) em nomes de arquivos ou extensões que você especificar ao usar esse comando. Para pesquisar uma cadeia de caracteres em um conjunto de arquivos que você especifica com curingas, você pode usar esse comando em um comando **for** .

- Se você usar **/c** e **/v** na mesma linha de comando, esse comando exibirá uma contagem das linhas que não contêm a cadeia de caracteres especificada. Se você especificar **/c** e **/n** na mesma linha de comando, **Find** irá ignorar **/n**.

- Este comando não reconhece retornos de carro. Quando você usa esse comando para Pesquisar texto em um arquivo que inclui retornos de carro, você deve limitar a cadeia de caracteres de pesquisa para texto que pode ser encontrado entre retornos de carro (ou seja, uma cadeia de caracteres que provavelmente não será interrompida por um retorno de carro). Por exemplo, esse comando não relatará uma correspondência para o arquivo de imposto sobre cadeia de caracteres se ocorrer um retorno de carro entre as palavras imposto e arquivo.

### <a name="examples"></a>Exemplos

Para exibir todas as linhas de *Pencil.ad* que contêm o *apontador de lápis*de cadeia de caracteres, digite:

```
find pencil sharpener pencil.ad
```

Para localizar o texto, "os cientistas rotularam seu papel apenas para fins de discussão. Não é um relatório final. " no arquivo *report.doc* , digite:

```
find ""The scientists labeled their paper for discussion only. It is not a final report."" report.doc
```

Para pesquisar um conjunto de arquivos, você pode usar o comando **Find** dentro do comando **for** . Para pesquisar o diretório atual em busca de arquivos que tenham a extensão. bat e que contenham o PROMPT de cadeia de caracteres, digite:

```
for %f in (*.bat) do find PROMPT %f
```

Para pesquisar o disco rígido para localizar e exibir os nomes de arquivo na unidade C que contém a CPU da cadeia de caracteres, use o pipe (|) para direcionar a saída do comando **dir** para o comando **Find** da seguinte maneira:

```
dir c:\ /s /b | find CPU
```

Como **Localizar** pesquisas diferencia maiúsculas de minúsculas e **dir** produz saída em maiúsculas, você deve digitar a CPU da cadeia de caracteres em letras maiúsculas ou usar a opção de linha de comando **/i** com **Localizar**.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando for](for.md)