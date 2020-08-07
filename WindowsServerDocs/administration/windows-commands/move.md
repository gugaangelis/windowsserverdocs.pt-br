---
title: move
description: Artigo de referência para o comando move, que move um ou mais arquivos de um diretório para outro diretório.
ms.topic: article
ms.assetid: fde290a8-d385-450f-8987-ee837fed667d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5ceeced7e734775138cc47cba9d36981a4433750
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87886321"
---
# <a name="move"></a>move

Move um ou mais arquivos de um diretório para outro diretório.

> [!IMPORTANT]
> Mover arquivos criptografados para um volume que não dá suporte aos resultados de Encrypting File System (EFS) resultará em um erro. Primeiro, você deve descriptografar os arquivos ou movê-los para um volume que ofereça suporte ao EFS.

## <a name="syntax"></a>Sintaxe

```
move [{/y|-y}] [<source>] [<target>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| /y | Interrompe a solicitação de confirmação de que você deseja substituir um arquivo de destino existente. Esse parâmetro pode ser predefinido na variável de ambiente COPYCMD. Você pode substituir essa predefinição usando o parâmetro **-y** . O padrão é solicitar antes de substituir arquivos, a menos que o comando seja executado de dentro de um script em lotes. |
| -y | Inicia a solicitação de confirmação de que você deseja substituir um arquivo de destino existente. |
| `<source>` | Especifica o caminho e o nome dos arquivos a serem movidos. Para mover ou renomear um diretório, a *origem* deve ser o nome e o caminho do diretório atual. |
| `<target>` | Especifica o caminho e o nome para os quais mover os arquivos. Para mover ou renomear um diretório, o *destino* deve ser o caminho e o nome do diretório desejado. |
| /? | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para mover todos os arquivos com a extensão. xls do diretório *\data* para o diretório *\ Second_Q \Reports* , digite:

```
move \data\*.xls \second_q\reports\
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
