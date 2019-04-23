---
title: find
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2ca66b22-3b7c-4166-8503-eb75fc53ab46
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b4639ff780687ad7a69ddba5374a722a15b06542
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848257"
---
# <a name="find"></a>find



Pesquisa uma cadeia de caracteres de texto em um arquivo ou arquivos e exibe as linhas de texto que contêm a cadeia de caracteres especificada.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
find [/v] [/c] [/n] [/i] [/off[line]] "<String>" [[<Drive>:][<Path>]<FileName>[...]]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|/v|Exibe todas as linhas que não contêm especificado \<cadeia de caracteres >.|
|/c|Conta as linhas que contêm especificado \<cadeia de caracteres > e exibe o total.|
|/n|Precede cada linha com o número da linha do arquivo.|
|/i|Especifica que a pesquisa não diferencia maiusculas de minúsculas.|
|[/off[line]]|Não ignorar arquivos que têm o atributo offline definido.|
|"\<String>"|Obrigatório. Especifica o grupo de caracteres (colocadas entre aspas) que você deseja pesquisar.|
|[\<Drive>:][<Path>]<FileName>|Especifica o local e o nome do arquivo no qual pesquisar a cadeia de caracteres especificada.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

-   Especificando uma cadeia de caracteres

    Se você não usar **/i**, **localizar** procura exatamente o que você especifica *cadeia de caracteres*. Por exemplo, o **localizar** comando trata os caracteres "a" e "A" forma diferente. Se você usar **/i**, no entanto, **localizar** não é diferencia maiusculas de minúsculas, e ele trata de "a" e "A" como o mesmo caractere.

    Se você deseja pesquisar a cadeia de caracteres contiver aspas, você deve usar aspas duplas para cada aspa contida na cadeia de caracteres (por exemplo, "esse""string" "contém aspas").
-   Usando o **localizar** como um filtro

    Se você omitir um nome de arquivo **encontrar** atua como um filtro, recebendo dados da origem padrão (geralmente o teclado, um pipe (|) ou um arquivo redirecionado) e, em seguida, exibindo todas as linhas que contêm *cadeia de caracteres*.
-   Ordenação de sintaxe de comando

    Você pode digitar parâmetros e opções de linha de comando para o **localizar** comando em qualquer ordem.
-   Usando caracteres curinga

    Não é possível usar caracteres curinga (**&#42;** e **?**) em nomes de arquivos ou extensões especificados com o **localizar** comando. Para procurar uma cadeia de caracteres em um conjunto de arquivos que você especificar com caracteres curinga, você pode usar o **encontrar** comando dentro de uma **para** comando.
-   Usando o **/v** ou **/n** com **/c**

    Se você usar **/c** e **/v** na mesma linha de comando **localizar** exibe uma contagem de linhas que não contêm a cadeia de caracteres especificada. Se você especificar **/c** e **/n** na mesma linha de comando **localizar** ignora **/n**.
-   Usando o **encontrar** com carro retorna

    O **localizar** comando não reconhece retornos de carro. Quando você usa **localizar** para pesquisar texto em um arquivo que inclui retornos de carro, você deve limitar a cadeia de caracteres de pesquisa para o texto que pode ser encontrado entre retornos de carro (ou seja, uma cadeia de caracteres que não seja provável de ser interrompida por um retorno de carro). Por exemplo, **localizar** não relata uma correspondência para a cadeia de caracteres "file imposto" se um retorno de carro ocorrer entre as palavras "" e "arquivo".

## <a name="BKMK_examples"></a>Exemplos

Para exibir todas as linhas do arquivo Lapis.AD que contêm a cadeia de caracteres "Apontador de lápis", digite:
```
find "Pencil Sharpener" pencil.ad
```
Para localizar uma cadeia de caracteres que contém o texto entre aspas, você deverá colocar toda a cadeia de caracteres entre aspas. Em seguida, você deve usar duas aspas para cada aspa contida na cadeia de caracteres. Para encontrar "Os cientistas de rotulado seu papel"somente para discussão." Não é um relatório final." no Report. doc, digite:
```
find "The scientists labeled their paper ""for discussion only."" It is not a final report." report.doc
```
Se você quiser pesquisar um conjunto de arquivos, você pode usar o **encontrar** comando dentro de **para** comando. Para pesquisar o diretório atual para arquivos que têm a extensão. bat e que contêm a cadeia de caracteres "PROMPT", digite:
```
for %f in (*.bat) do find "PROMPT" %f 
```
Para pesquisar o disco rígido para localizar e exibir os nomes de arquivo na unidade C que contêm a cadeia de caracteres "CPU", use a barra vertical (|) para direcionar a saída do **dir** comando para o **localizar** comando da seguinte maneira:
```
dir c:\ /s /b | find "CPU" 
```
Porque **encontrar** pesquisas diferenciam maiusculas de minúsculas e **dir** produz saída de letras maiusculas, você deve digitar a cadeia de caracteres "CPU" com letras maiusculas ou usar o **/i** de linha de comando a opção com **localizar**.

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)