---
title: clip
description: Artigo de referência para o comando de clipe, que redireciona a saída de comando da linha de comando para a área de transferência do Windows.
ms.topic: article
ms.assetid: 85322d85-3376-4806-845b-93ac77fe27bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ee6527fd66678d58e971eb12e3cb92724d50517d
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880121"
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