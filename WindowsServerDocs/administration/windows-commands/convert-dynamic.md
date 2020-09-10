---
title: convert dynamic
description: Artigo de referência para o comando Convert dinâmico, que converte um disco básico em um disco dinâmico.
ms.topic: reference
ms.assetid: 7b8fa4b1-850f-4e48-b05f-871c883ea33c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7a67b7e900ce57e7551d8c565022aa80be5d714a
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89629352"
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
