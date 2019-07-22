---
title: chamada
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d34a41dc-e6c7-4467-bf6a-15cec704833e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 1f5253700f2932b2afa725163121e64ea4c1748d
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66434585"
---
# <a name="call"></a>chamada



Chama um arquivo em lotes de outro sem interromper o programa de lote do pai. O **chamar** comando aceita rótulos como o destino da chamada.

> [!NOTE]
> **Chamar** não tem nenhum efeito no prompt de comando quando ele é usado fora de um arquivo de script ou lote.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
call [Drive:][Path]<FileName> [<BatchParameters>] [:<Label> [<Arguments>]]
```

## <a name="parameters"></a>Parâmetros

|           Parâmetro           |                                                                         Descrição                                                                          |
|-------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:][<Path>]<FileName> | Especifica o local e o nome do programa em lote que você deseja chamar. O *FileName* parâmetro é obrigatório e deve ter uma extensão. bat ou. cmd. |
|      \<Parâmetros_em_lotes >       |                                            Especifica qualquer informação de linha de comando necessária para o arquivo em lotes.                                             |
|           :\<Label>           |                                            Especifica o rótulo que você deseja que um controle do programa de lote para ir para.                                             |
|         \<Arguments>          |                     Especifica as informações de linha de comando a serem passados para a nova instância de lotes, começando em *: rótulo.*                     |
|              /?               |                                                             Exibe a ajuda no prompt de comando.                                                             |

## <a name="batch-parameters"></a>Parâmetros do lote

As referências de argumento de script em lotes ( **%0**, **%1**,...) são listados nas tabelas a seguir.

**%** * em um lote script refere-se a todos os argumentos (por exemplo, **%1**, **%2**, **%3**...)

Você pode usar as seguintes sintaxes opcionais como substituições para os parâmetros do lote ( **%n**):

|Parâmetro de lote|Descrição|
|---------------|-----------|
|%~1|Expande **%1** e remove ao redor das aspas ("").|
|%~f1|Expande **%1** para um caminho totalmente qualificado.|
|%~d1|Expande **%1** para uma letra de unidade.|
|%~p1|Expande **%1** para somente um caminho.|
|%~n1|Expande **%1** para um nome de arquivo.|
|%~x1|Expande **%1** para apenas uma extensão de nome de arquivo.|
|%~s1|Expande **%1** para um caminho totalmente qualificado que contém somente nomes curtos.|
|%~a1|Expande **%1** para os atributos de arquivo.|
|%~t1|Expande **%1** como a data e hora do arquivo.|
|%~z1|Expande **%1** para o tamanho do arquivo.|
|%~$PATH:1|Pesquisa as pastas listadas na variável de ambiente PATH e expande **%1** para o nome totalmente qualificado do diretório primeiro encontrado. Se o nome da variável de ambiente não está definido ou o arquivo não for encontrado pela pesquisa, esse modificador expande a cadeia de caracteres vazia.|

A tabela a seguir mostra como você pode combinar os modificadores com os parâmetros de lote para resultados compostos:

|Parâmetro de lote com o modificador|Descrição|
|-----------------------------|-----------|
|%~dp1|Expande **%1** para uma letra de unidade e caminho apenas.|
|%~nx1|Expande **%1** para um nome de arquivo e extensão somente.|
|%~dp$PATH:1|Pesquisa as pastas listadas na variável de ambiente PATH **%1**e, em seguida, expande para a letra da unidade e caminho do diretório a primeira encontrado.|
|%~ftza1|Expande **%1** para exibir a saída semelhante ao **dir** comando.|

Nos exemplos acima, **%1** e o caminho pode ser substituído por outros valores válidos. O <strong>%~</strong> sintaxe é terminada por um número de argumento válido. O <strong>%~</strong> modificadores não podem ser usados com **%\\** *.

## <a name="remarks"></a>Comentários

-   Usando parâmetros em lotes

    Parâmetros de lote podem conter todas as informações que você pode passar para um programa em lote, incluindo opções de linha de comando, nomes de arquivo, os parâmetros do lote **%0** por meio **%9**e as variáveis (por exemplo, **% baud %** ).
-   Usando o *rótulo* parâmetro

    Usando **chamar** com o *rótulo* parâmetro, crie um novo contexto de arquivo em lotes e passar o controle para a instrução após o rótulo especificado. Na primeira vez em que o final do arquivo em lotes for encontrado (isto é, afinal saltar para o rótulo), o controle retorna para a instrução após o **chamar** instrução. Na segunda vez que o final do arquivo em lotes for encontrado, o script em lotes é encerrado.
-   Usando pipes e símbolos de redirecionamento

    Não use pipes ( **|** ) e símbolos de redirecionamento ( **<** ou **>** ) com **chamar**.
-   Fazendo uma chamada recursiva

    Você pode criar um arquivo em lotes que chama a mesmo. No entanto, você deve fornecer uma condição de saída. Caso contrário, os programas de lote pai e filho podem permanecer em loop indefinidamente.
-   Trabalhando com as extensões de comando

    Se as extensões de comando estiverem habilitadas, **chamar** aceita *rótulo* como o destino da chamada. A sintaxe correta é da seguinte maneira:

    `call :\<Label> <Arguments>`

## <a name="BKMK_examples"></a>Exemplos

Para executar o arquivo Verifnov de outro arquivo em lotes, digite o comando a seguir no arquivo em lotes pai:
```
call checknew
```
Se o arquivo em lotes pai aceita dois parâmetros de lote e você queira passar esses parâmetros para Verifnov, digite o comando a seguir no arquivo em lotes pai:
```
call checknew %1 %2
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)