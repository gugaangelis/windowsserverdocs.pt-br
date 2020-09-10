---
title: clip
description: Artigo de referência para o comando de clipe, que redireciona a saída de comando da linha de comando para a área de transferência do Windows.
ms.topic: reference
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 9f6820d60a8c94ed07bbb23bbbd86be3f1f289b7
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629623"
---
# <a name="clip"></a>clip

Redireciona a saída do comando da linha de comando para a área de transferência do Windows. Você pode usar esse comando para copiar dados diretamente em qualquer aplicativo que possa receber texto da área de transferência. Você também pode colar essa saída de texto em outros programas.

## <a name="syntax"></a>Sintaxe

```
<command> | clip
clip < <filename>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| `<command>` | Especifica um comando cuja saída você deseja enviar para a área de transferência do Windows. |
| `<filename>` | Especifica um arquivo cujo conteúdo você deseja enviar para a área de transferência do Windows. |
| /? | Exibe a ajuda no prompt de comando. |

## <a name="examples"></a>Exemplos

Para copiar a listagem de diretório atual para a área de transferência do Windows, digite:

```
dir | clip
```

Para copiar a saída de um programa chamado *Generic. awk* para a área de transferência do Windows, digite:

```
awk -f generic.awk input.txt | clip
```

Para copiar o conteúdo de um arquivo chamado *readme.txt* para a área de transferência do Windows, digite:

```
clip < readme.txt
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)