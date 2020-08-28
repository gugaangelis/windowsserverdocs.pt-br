---
title: convert dynamic
description: Artigo de referência para o comando Convert dinâmico, que converte um disco básico em um disco dinâmico.
ms.topic: reference
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 015fba655c4e57345ca457a5901ba17ed38b5cf3
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025900"
---
# <a name="convert-dynamic"></a>convert dynamic

Converte um disco básico em um disco dinâmico. Um disco básico deve ser selecionado para que essa operação tenha sucesso. Use o [comando selecionar disco](select-disk.md) para selecionar um disco básico e deslocar o foco para ele.

> [!NOTE]
> Para obter instruções sobre como usar esse comando, consulte [alterar um disco dinâmico de volta para um disco básico](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc755238(v=ws.11))).

## <a name="syntax"></a>Sintaxe

```
convert dynamic [noerr]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |

#### <a name="remarks"></a>Comentários

- Todas as partições existentes no disco básico se tornam volumes simples.

## <a name="examples"></a>Exemplos

Para converter um disco básico em um disco dinâmico, digite:

```
convert dynamic
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Convert](convert.md)
