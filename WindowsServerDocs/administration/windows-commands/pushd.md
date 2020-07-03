---
title: pushd
description: Artigo de referência para o comando PUSHD, que armazena o diretório atual para uso pelo comando POPD e, em seguida, muda para o diretório especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 396bc545-0f41-473e-b0ac-76fbbb74d390
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 871834ae1ac29eb53be982831e7ede93d9d309cf
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85933752"
---
# <a name="pushd"></a>pushd

Armazena o diretório atual para uso pelo comando **popd** e, em seguida, altera para o diretório especificado.

Sempre que você usa o comando **PUSHD** , um único diretório é armazenado para seu uso. No entanto, você pode armazenar vários diretórios usando o comando **PUSHD** várias vezes. Os diretórios são armazenados em sequência em uma pilha virtual, portanto, se você usar o comando **PUSHD** uma vez, o diretório no qual você usa o comando será colocado na parte inferior da pilha. Se você usar o comando novamente, o segundo diretório será colocado na parte superior do primeiro. O processo é repetido sempre que você usa o comando **PUSHD** .

Se você usar o comando **popd** , o diretório na parte superior da pilha será removido e o diretório atual será alterado para esse diretório. Se você usar o comando **popd** novamente, o próximo diretório na pilha será removido. Se as extensões de comando estiverem habilitadas, o comando **popd** removerá as atribuições de letra de unidade criadas pelo comando **PUSHD** .

## <a name="syntax"></a>Sintaxe

```
pushd [<path>]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| `<path>` | Especifica o diretório para tornar o diretório atual. Este comando dá suporte a caminhos relativos. |
| /? | Exibe a ajuda no prompt de comando. |

#### <a name="remarks"></a>Comentários

- Se as extensões de comando estiverem habilitadas, o comando **PUSHD** aceitará um caminho de rede ou uma letra de unidade local e um caminho.

- Se você especificar um caminho de rede, o comando **Pushed** atribuirá temporariamente a letra de unidade não utilizada mais alta (começando com Z:) para o recurso de rede especificado. O comando altera a unidade e o diretório atuais para o diretório especificado na unidade atribuída recentemente. Se você usar o comando **popd** com extensões de comando habilitadas, o comando **popd** removerá a atribuição de letra da unidade criada por **PUSHD**.

### <a name="examples"></a>Exemplos

Para alterar o diretório atual a partir daquele em que o programa do lote foi executado e, em seguida, para alterá-lo novamente:

```
@echo off
rem This batch file deletes all .txt files in a specified directory
pushd %1
del *.txt
popd
cls
echo All text files deleted in the %1 directory
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando POPD](popd.md)
