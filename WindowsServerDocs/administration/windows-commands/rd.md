---
title: rd
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 94231e3ec032280beb91a14db7949a1296c2d811
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442034"
---
# <a name="rd"></a>rd



Exclui um diretório. Esse comando é igual a **rmdir** comando.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

## <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                                 Descrição                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [\<Drive>:]<Path> |                      Especifica o local e o nome do diretório que você deseja excluir. *Caminho* é necessária.                       |
|        /s         |                     Exclui uma árvore de diretório (o diretório especificado e todos os seus subdiretórios, incluindo todos os arquivos).                      |
|        /q         | Especifica o modo silencioso. Não solicita confirmação quando a exclusão de uma árvore de diretório. (Observe que **/q** funciona apenas se **/s** for especificado.) |
|        /?         |                                                     Exibe a ajuda no prompt de comando.                                                     |

## <a name="remarks"></a>Comentários

-   Você não pode excluir um diretório que contém os arquivos, incluindo ocultos ou arquivos de sistema. Se você tentar fazer isso, a seguinte mensagem será exibida:

    `The directory is not empty`

    Use o **dir /a** comando para listar todos os arquivos (incluindo ocultos e arquivos do sistema). Em seguida, use o **attrib** com **-h** para remover os atributos de arquivo oculto **-s** para remover os atributos de arquivo do sistema, ou **-h -s** para remover ambos ocultos e os atributos de arquivo do sistema. Após o oculto e atributos de arquivo foram removidos, você pode excluir os arquivos.
-   Se você inserir uma barra invertida (\) no início de *caminho*, *caminho* começará no diretório raiz (independentemente do diretório atual).
-   Não é possível usar **área de trabalho remota** para excluir o diretório atual. Se você tentar excluir o diretório atual, a seguinte mensagem de erro será exibida:

    `The process cannot access the file because it is being used by another process.`

    Se você receber essa mensagem de erro, você deve alterar para um diretório diferente (não um subdiretório do diretório atual) e, em seguida, use **área de trabalho remota** (especificar *caminho* se necessário).
-   O **área de trabalho remota** comando com parâmetros diferentes, está disponível no Console de recuperação.

## <a name="BKMK_examples"></a>Exemplos

É possível excluir o diretório que você está trabalhando no momento. Você deve alterar para um diretório que não está dentro do diretório atual. Por exemplo, para alterar para o diretório pai, digite:
```
cd ..
```
Agora você pode remover com segurança no diretório desejado.

Use o **/s** opção para remover uma árvore de diretório. Por exemplo, para remover um diretório chamado teste (e todas as suas subpastas e arquivos) do diretório atual, digite:
```
rd /s test
```
Para executar o exemplo anterior no modo silencioso, digite:
```
rd /s /q test
```

> [!CAUTION]
> Quando você executa **rd /s** no modo silencioso, toda a árvore será excluída sem confirmação. Certifique-se de que arquivos importantes são movidos ou em backup antes de usar o **/q** opção de linha de comando.

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)