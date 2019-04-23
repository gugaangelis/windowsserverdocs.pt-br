---
title: clip
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: b10876e115c1f0dcac3448948003852449012087
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862387"
---
# <a name="clip"></a>clip



Redireciona a saída de comando da linha de comando para a área de transferência do Windows. Você pode colar essa saída de texto em outros programas.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
<Command> | clip
clip < <FileName>
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|\<Comando >|Especifica um comando cuja saída você deseja enviar para a área de transferência do Windows.|
|\<FileName>|Especifica um arquivo cujo conteúdo você deseja enviar para a área de transferência do Windows.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Você pode usar o **clip** comando para copiar dados diretamente em qualquer aplicativo que possa receber o texto da área de transferência.

## <a name="BKMK_examples"></a>Exemplos

Para copiar o diretório atual, a listagem na área de transferência do Windows, digite:
```
dir | clip
```
Para copiar a saída de um programa chamado Generic. awk na área de transferência do Windows, digite:
```
awk -f generic.awk input.txt | clip
```
Para copiar o conteúdo de um arquivo chamado Readme. txt para a área de transferência do Windows, digite:
```
clip < readme.txt
```

#### <a name="additional-references"></a>Referências adicionais

[Chave de sintaxe de linha de comando](command-line-syntax-key.md)