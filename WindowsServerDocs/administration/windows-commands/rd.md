---
title: rd
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 42e672f6-5bc2-4c16-af25-18e7ed2dd555
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 298e6b291a6aa08701b6d54a11470b0cc4bea486
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836709"
---
# <a name="rd"></a>rd



Exclui um diretório. Esse comando é o mesmo que o comando **rmdir** .

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
rd [<Drive>:]<Path> [/s [/q]]
rmdir [<Drive>:]<Path> [/s [/q]]
```

### <a name="parameters"></a>Parâmetros

|     Parâmetro     |                                                                 Descrição                                                                  |
|-------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| [\<drive >:]<Path> |                      Especifica o local e o nome do diretório que você deseja excluir. O *caminho* é necessário.                       |
|        /s         |                     Exclui uma árvore de diretórios (o diretório especificado e todos os seus subdiretórios, incluindo todos os arquivos).                      |
|        /q         | Especifica o modo silencioso. Não solicita confirmação ao excluir uma árvore de diretórios. (Observe que **/q** só funcionará se **/s** for especificado.) |
|        /?         |                                                     Exibe a ajuda no prompt de comando.                                                     |

## <a name="remarks"></a>Comentários

-   Você não pode excluir um diretório que contém arquivos, incluindo arquivos ocultos ou do sistema. Se você tentar fazer isso, a seguinte mensagem será exibida:

    `The directory is not empty`

    Use o comando **dir/a** para listar todos os arquivos (incluindo arquivos ocultos e do sistema). Em seguida, use o comando **attrib** com **-h** para remover atributos de arquivo oculto, **-s** para remover atributos de arquivo do sistema ou **-h-s** para remover os atributos de arquivo ocultos e do sistema. Depois que os atributos ocultos e de arquivo forem removidos, você poderá excluir os arquivos.
-   Se você inserir uma barra invertida (\) no início do *caminho*, o *caminho* será iniciado no diretório raiz (independentemente do diretório atual).
-   Você não pode usar o **RD** para excluir o diretório atual. Se você tentar excluir o diretório atual, a seguinte mensagem de erro será exibida:

    `The process cannot access the file because it is being used by another process.`

    Se você receber essa mensagem de erro, deverá alterar para um diretório diferente (não um subdiretório do diretório atual) e, em seguida, usar **RD** (especifique o *caminho* , se necessário).
-   O comando **RD** , com parâmetros diferentes, está disponível no console de recuperação.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

Não é possível excluir o diretório no qual você está trabalhando no momento. Você deve alterar para um diretório que não esteja dentro do diretório atual. Por exemplo, para alterar para o diretório pai, digite:
```
cd ..
```
Agora você pode remover com segurança o diretório desejado.

Use a opção **/s** para remover uma árvore de diretórios. Por exemplo, para remover um diretório chamado Test (e todos os seus arquivos e subdiretórios) do diretório atual, digite:
```
rd /s test
```
Para executar o exemplo anterior no modo silencioso, digite:
```
rd /s /q test
```

> [!CAUTION]
> Quando você executa o **RD/s** no modo silencioso, toda a árvore de diretórios é excluída sem confirmação. Certifique-se de que arquivos importantes sejam movidos ou submetidos a backup antes de usar a opção de linha de comando **/q** .

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)