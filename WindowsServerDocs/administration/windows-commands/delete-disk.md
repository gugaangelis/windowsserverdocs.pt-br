---
title: delete disk
description: Artigo de referência do comando excluir disco, que exclui um disco dinâmico ausente da lista de discos.
ms.topic: reference
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: f26bd239c1248630da4ce684547961a239f96b8e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89628866"
---
# <a name="delete-disk"></a>delete disk

Exclui um disco dinâmico ausente da lista de discos.

> [!NOTE]
> Para obter instruções detalhadas sobre como usar esse comando, consulte [remover um disco dinâmico ausente](/previous-versions/windows/it-pro/windows-server-2008-r2-and-2008/cc753029(v=ws.11)).

## <a name="syntax"></a>Sintaxe

```
delete disk [noerr] [override]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| NOERR | Somente para scripts. Quando um erro é encontrado, o DiskPart continua processando comandos como se o erro não tivesse ocorrido. Sem esse parâmetro, um erro faz com que o DiskPart saia com um código de erro. |
| override | Permite que o DiskPart exclua todos os volumes simples no disco. Se o disco contiver metade de um volume espelhado, a metade do espelho no disco será excluída. O comando excluir substituição de disco falhará se o disco for membro de um volume RAID-5. |

## <a name="examples"></a>Exemplos

Para excluir um disco dinâmico ausente da lista de discos, digite:

```
delete disk
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Excluir comando](delete.md)
