---
title: popd
description: Saiba como alterar o diretório para o diretório armazenado mais recentemente pelo comando Pushed.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: a9cf2814afcab3e6d7373642ad5bf8bc828bf07b
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821216"
---
# <a name="popd"></a>popd

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o diretório atual para o diretório que foi armazenado mais recentemente pelo comando **Pushed** .


## <a name="syntax"></a>Sintaxe
```
popd
```

#### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   Sempre que você usa o comando **PUSHD** , um único diretório é armazenado para seu uso. No entanto, você pode armazenar vários diretórios usando o comando **PUSHD** várias vezes.
    Os diretórios são armazenados em sequência em uma pilha virtual. Se você usar o comando **PUSHD** uma vez, o diretório no qual você usa o comando será colocado na parte inferior da pilha. Se você usar o comando novamente, o segundo diretório será colocado na parte superior do primeiro. O processo é repetido sempre que você usa o comando **PUSHD** .
    Você pode usar o comando **popd** para alterar o diretório atual para o diretório armazenado mais recentemente pelo comando **Pushed** . Se você usar o comando **popd** , o diretório na parte superior da pilha será removido da pilha e o diretório atual será alterado para esse diretório. Se você usar o comando **popd** novamente, o próximo diretório na pilha será removido.
-   Quando as extensões de comando são habilitadas, o comando **popd** remove qualquer atribuição de letra de unidade criada por **PUSHD**.

## <a name="examples"></a><a name="BKMK_examples"></a>Exemplos
Para mostrar como você pode usar o comando **Pushed** e o comando **popd** em um programa em lotes para alterar o diretório atual daquele em que o programa do lote foi executado e, em seguida, alterá-lo novamente:

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
-   [pushd](pushd.md)
- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

