---
title: delete disk
description: Artigo de referência do comando excluir disco, que exclui um disco dinâmico ausente da lista de discos.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 44079900-e4ed-49d0-81e4-d652c38cd636
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55eabb19e10a7ca6fe4dd705ab91961919bb2b7c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86958398"
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
