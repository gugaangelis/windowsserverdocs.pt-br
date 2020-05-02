---
title: chamada
description: Tópico de referência para o comando Call, que chama um programa em lotes de outro sem parar o programa do lote pai.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d34a41dc-e6c7-4467-bf6a-15cec704833e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 06/05/2018
ms.openlocfilehash: 64c4b89d18ab869a7e6c8b1ee8537c4f808bce8f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82719664"
---
# <a name="call"></a>chamada

Chama um programa em lotes de outro sem interromper o programa do lote pai. O comando **Call** aceita rótulos como o destino da chamada

> [!NOTE]
> A chamada não tem nenhum efeito no prompt de comando quando ele é usado fora de um script ou arquivo em lotes.

## <a name="syntax"></a>Sintaxe

```
call [drive:][path]<filename> [<batchparameters>] [:<label> [<arguments>]]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `[<drive>:][<path>]<filename>` | Especifica o local e o nome do programa em lotes que você deseja chamar. O `<filename>` parâmetro é obrigatório e deve ter uma extensão. bat ou. cmd. |
| `<batchparameters>` | Especifica qualquer informação de linha de comando exigida pelo programa em lotes. |
| `:<label>` | Especifica o rótulo para o qual você deseja que um controle de programa do lote salte. |
| `<arguments>` | Especifica as informações de linha de comando a serem passadas para a nova instância do programa em lotes, `:<label>`começando em.|
| /? | Exibe a ajuda no prompt de comando. |

## <a name="batch-parameters"></a>Parâmetros de lote

As referências de argumento de script de lote (**%0**, **%1**,...) estão listadas nas tabelas a seguir.

Usar o valor **% &#42;** em um script em lotes refere-se a todos os argumentos (por exemplo, **%1**, **%2**, **%3**...).

Você pode usar as seguintes sintaxes opcionais como substituições para parâmetros de lote (**% n**):

| Parâmetro de lote | Descrição |
| --------------- | ----------- |
| % ~ 1 | Expande **%1** e remove aspas ao redor. |
| % ~ F1 | Expande **%1** para um caminho totalmente qualificado. |
| % ~ D1 | Expande **%1** somente para uma letra de unidade. |
| % ~ P1 | Expande **%1** somente para um caminho. |
| % ~ N1 | Expande **%1** somente para um nome de arquivo. |
| % ~ X1 | Expande **%1** para uma extensão de nome de arquivo somente. |
| % ~ S1 | Expande **%1** para um caminho totalmente qualificado que contém apenas nomes curtos. |
| % ~ a1 | Expande **%1** para os atributos do arquivo. |
| % ~ T1 | Expande **%1** para a data e hora do arquivo. |
| % ~ Z1 | Expande **%1** para o tamanho do arquivo. |
| % ~ $PATH: 1 | Pesquisa os diretórios listados na variável de ambiente PATH e expande **%1** para o nome totalmente qualificado do primeiro diretório encontrado. Se o nome da variável de ambiente não for definido ou o arquivo não for encontrado pela pesquisa, esse modificador se expandirá para a cadeia de caracteres vazia. |

A tabela a seguir mostra como você pode combinar modificadores com os parâmetros de lote para resultados compostos:

| Parâmetro de lote com modificador | Descrição |
| ----------------------------- | ----------- |
| % ~ DP1 | Expande **%1** somente para uma letra de unidade e um caminho. |
| % ~ NX1 | Expande **%1** somente para um nome de arquivo e extensão. |
| % ~ DP $ caminho: 1 | Pesquisa os diretórios listados na variável de ambiente PATH para **%1**e, em seguida, expande para a letra da unidade e o caminho do primeiro diretório encontrado. |
| % ~ ftza1 | Expande **%1** para exibir uma saída semelhante ao comando **dir** . |

Nos exemplos acima, **%1** e o caminho podem ser substituídos por outros valores válidos. A **%~** sintaxe é encerrada por um número de argumento válido. Os **%~** modificadores não podem ser usados com **% &#42;**.

### <a name="remarks"></a>Comentários

- Usando parâmetros de lote:

    Os parâmetros de lote podem conter qualquer informação que você possa passar para um programa em lotes, incluindo opções de linha de comando, nomes de arquivo, os parâmetros de lote **%0** a **%9**e variáveis (por exemplo, **% baud%**).

- Usando o `<label>` parâmetro:

    Ao usar **Call** com o `<label>` parâmetro, você cria um novo contexto de arquivo em lotes e passa o controle para a instrução após o rótulo especificado. Na primeira vez que o final do arquivo em lotes é encontrado (ou seja, após saltar para o rótulo), o controle retorna à instrução após a instrução **Call** . Na segunda vez que o final do arquivo em lotes for encontrado, o script em lotes será encerrado.

- Usando pipes e símbolos de redirecionamento:

    Não use pipes `(|)` ou símbolos de redirecionamento`<` ( `>`ou) com **Call**.

- Fazendo uma chamada recursiva

    Você pode criar um programa em lotes que chama a si mesmo. No entanto, você deve fornecer uma condição de saída. Caso contrário, os programas de lote pai e filho podem fazer loops informativos.

- Trabalhando com extensões de comando

    Se as extensões de comando estiverem **call** habilitadas `<label>` , chame aceita como o destino da chamada. A sintaxe correta é`call :<label> <arguments>`

## <a name="examples"></a>Exemplos

Para executar o programa checknew. bat de outro programa em lotes, digite o seguinte comando no programa do lote pai:

```
call checknew
```

Se o programa do lote pai aceitar dois parâmetros de lote e você quiser que ele passe esses parâmetros para checknew. bat, digite o seguinte comando no programa do lote pai:

```
call checknew %1 %2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
