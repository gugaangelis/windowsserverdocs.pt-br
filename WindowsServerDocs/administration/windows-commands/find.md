---
title: find
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82b966db4117e9273ae6aed8d30baec76362de2f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80844689"
---
# <a name="find"></a>find



Pesquisa uma cadeia de caracteres de texto em um ou mais arquivos e exibe linhas de texto que contêm a cadeia de caracteres especificada.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
find [/v] [/c] [/n] [/i] [/off[line]] <String> [[<Drive>:][<Path>]<FileName>[...]]
```

### <a name="parameters"></a>Parâmetros

|           Parâmetro           |                                              Descrição                                               |
|-------------------------------|--------------------------------------------------------------------------------------------------------|
|              /v               |                    Exibe todas as linhas que não contêm a cadeia de caracteres de \<especificada >.                     |
|              /c               |              Conta as linhas que contêm a cadeia de caracteres de \<especificada > e exibe o total.              |
|              /n               |                            Precede cada linha com o número de linha do arquivo.                             |
|              /i               |                            Especifica que a pesquisa não diferencia maiúsculas de minúsculas.                            |
|         [/off [line]]          |                        Não ignora arquivos que têm o atributo offline definido.                        |
|          Cadeia de caracteres \<>          | Obrigatório. Especifica o grupo de caracteres (entre aspas) que você deseja pesquisar. |
| [\<drive >:] [<Path>]<FileName> |        Especifica o local e o nome do arquivo no qual Pesquisar a cadeia de caracteres especificada.        |
|              /?               |                                  Exibe a ajuda no prompt de comando.                                  |

## <a name="remarks"></a>Comentários

-   Especificando uma cadeia de caracteres

    Se você não usar **/i**, **Find** procurará exatamente o que você especificar para a *cadeia de caracteres*. Por exemplo, o comando **Find** trata os caracteres a e diferente. No entanto, se você usar **/i**, **Find** não diferenciará maiúsculas de minúsculas e tratará um e um como o mesmo caractere.

    Se a cadeia de caracteres que você deseja pesquisar contiver aspas, você deverá usar aspas duplas para cada aspa contida na cadeia de caracteres (por exemplo, essa cadeia de caracteres contém aspas).
-   Usando **Find** como um filtro

    Se você omitir um nome de arquivo, **Find** atua como um filtro, tomando entrada da fonte de entrada padrão (geralmente o teclado, um pipe (|) ou um arquivo redirecionado) e, em seguida, exibindo todas as linhas que contêm uma *cadeia de caracteres*.
-   Sintaxe de comando de ordenação

    Você pode digitar parâmetros e opções de linha de comando para o comando **Find** em qualquer ordem.
-   Usando curingas

    Você não pode usar caracteres curinga **&#42;** (e **?** ) em nomes de arquivos ou extensões que você especificar com o comando **Localizar** . Para pesquisar uma cadeia de caracteres em um conjunto de arquivos que você especifica com curingas, você pode usar o comando **Find** dentro de um comando **for** .
-   Usando **/v** ou **/n** com **/c**

    Se você usar **/c** e **/v** na mesma linha de comando, **Find** exibirá uma contagem das linhas que não contêm a cadeia de caracteres especificada. Se você especificar **/c** e **/n** na mesma linha de comando, **Find** irá ignorar **/n**.
-   Usando **Find** com retornos de carro

    O comando **Find** não reconhece retornos de carro. Quando você usa **Localizar** para Pesquisar texto em um arquivo que inclui retornos de carro, você deve limitar a cadeia de caracteres de pesquisa ao texto que pode ser encontrado entre retornos de carro (ou seja, uma cadeia de caracteres que provavelmente não será interrompida por um retorno de carro). Por exemplo, **Localizar** não relatará uma correspondência para o arquivo de imposto sobre cadeia de caracteres se ocorrer um retorno de carro entre as palavras imposto e arquivo.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Para exibir todas as linhas de Pencil.ad que contêm o apontador de lápis de cadeia de caracteres, digite:
```
find Pencil Sharpener pencil.ad
```
Para localizar uma cadeia de caracteres que contém o texto entre aspas, você deve colocar a cadeia de caracteres inteira entre aspas. Em seguida, você deve usar duas aspas para cada aspa contida na cadeia de caracteres. Para achar que os cientistas rotularam seu papel apenas para fins de discussão. Não é um relatório final. em Report. doc, digite:
```
find The scientists labeled their paper for discussion only. It is not a final report. report.doc
```
Se você quiser pesquisar um conjunto de arquivos, poderá usar o comando **Find** dentro do comando **for** . Para pesquisar o diretório atual em busca de arquivos que tenham a extensão. bat e que contenham o PROMPT de cadeia de caracteres, digite:
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