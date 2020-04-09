---
title: clip
description: Tópico de comandos do Windows para o clip, que redireciona a saída do comando da linha de comando para a área de transferência do Windows.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d997154a382cf39aa2b877d7a2b84f4ff34157d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80847639"
---
# <a name="clip"></a>clip

Redireciona a saída do comando da linha de comando para a área de transferência do Windows. Em seguida, você pode colar essa saída de texto em outros programas.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

## <a name="syntax"></a>Sintaxe

```
<Command> | clip
clip < <FileName>
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|> de comando \<|Especifica um comando cuja saída você deseja enviar para a área de transferência do Windows.|
|\<nome de arquivo >|Especifica um arquivo cujo conteúdo você deseja enviar para a área de transferência do Windows.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Você pode usar o comando de **clipe** para copiar dados diretamente em qualquer aplicativo que possa receber texto da área de transferência.

## <a name="examples"></a><a name=BKMK_examples></a>Disso

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

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)