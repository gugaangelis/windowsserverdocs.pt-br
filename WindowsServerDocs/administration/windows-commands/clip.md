---
title: clip
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 82186869782c47f41930d46b4c33a710e6addedf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379355"
---
# <a name="clip"></a>clip



Redireciona a saída do comando da linha de comando para a área de transferência do Windows. Em seguida, você pode colar essa saída de texto em outros programas.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Command >|Especifica um comando cuja saída você deseja enviar para a área de transferência do Windows.|
|\<Nome de arquivo >|Especifica um arquivo cujo conteúdo você deseja enviar para a área de transferência do Windows.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Você pode usar o comando de **clipe** para copiar dados diretamente em qualquer aplicativo que possa receber texto da área de transferência.

## <a name="BKMK_examples"></a>Disso

Para copiar a listagem de diretório atual para a área de transferência do Windows, digite:
```
dir | clip
```
Para copiar a saída de um programa chamado Generic. awk para a área de transferência do Windows, digite:
```
awk -f generic.awk input.txt | clip
```
Para copiar o conteúdo de um arquivo chamado README. txt para a área de transferência do Windows, digite:
```
clip < readme.txt
```

#### <a name="additional-references"></a>Referências adicionais

[Chave da sintaxe de linha de comando](command-line-syntax-key.md)