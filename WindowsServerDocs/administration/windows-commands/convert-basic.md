---
title: convert basic
description: Artigo de referência para o comando Convert Basic, que converte um disco dinâmico vazio em um disco básico.
ms.topic: article
ms.assetid: 61329896-3b56-4959-8d58-45cbe18ba860
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 88ad686cd47bc9c347469697511a81f6cf4ae835
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87892591"
---
# <a name="convert-basic"></a>convert basic

Converte um disco dinâmico vazio em um disco básico. Um disco dinâmico deve ser selecionado para que essa operação tenha sucesso. Use o [comando selecionar disco](select-disk.md) para selecionar um disco dinâmico e deslocar o foco para ele.

> [!IMPORTANT]
> O disco deve estar vazio para convertê-lo em um disco básico. Faça backup dos dados e exclua todas as partições ou volumes antes de converter o disco.

> [!NOTE]
> Para obter instruções sobre como usar esse comando, consulte [alterar um disco dinâmico de volta para um disco básico](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755238(v=ws.11))).

## <a name="syntax"></a>Sintaxe

```
convert basic [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

## <a name="examples"></a>Exemplos

Para converter o disco dinâmico selecionado em básico, digite:

```
convert basic
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Convert](convert.md)
