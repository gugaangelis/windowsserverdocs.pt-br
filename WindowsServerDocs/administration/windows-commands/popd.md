---
title: popd
description: Saiba como alterar o diretório para a pasta mais recentemente armazenada pelo comando pushd.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8a4c52d5-9fd1-4eac-9c0c-5767b25728ed
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 6da6dc9d1fc2d8965f8a081831cb1150375209a4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827457"
---
# <a name="popd"></a>popd

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o diretório atual para o diretório que recentemente foi armazenado pela **pushd** comando.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe
```
popd
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   Toda vez que usar o **pushd** de comando, um único diretório é armazenado para seu uso. No entanto, você pode armazenar vários diretórios usando o **pushd** comando várias vezes.
    Os diretórios são armazenados em sequência em uma pilha virtual. Se você usar o **pushd** uma vez, o diretório em que você use o comando é colocado na parte inferior da pilha de comando. Se você usar o comando novamente, a segunda pasta será colocada sobre o primeiro deles. O processo se repete toda vez que usar o **pushd** comando.
    Você pode usar o **popd** comando para alterar o diretório atual para o diretório armazenado mais recentemente pelo **pushd** comando. Se você usar o **popd** de comando, o diretório no topo da pilha é removido da pilha e o diretório atual é alterado para esse diretório. Se você usar o **popd** comando novamente, a próxima pasta na pilha é removida.
-   Quando as extensões de comando estão habilitadas, o **popd** comando remove quaisquer atribuições de letra de unidade criadas pelo **pushd**.

## <a name="BKMK_examples"></a>Exemplos
O exemplo a seguir mostra como você pode usar o **pushd** comando e o **popd** comando em um arquivo em lotes para alterar o diretório atual no qual o arquivo em lotes foi executado e, em seguida, reverter essa alteração:

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
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)

