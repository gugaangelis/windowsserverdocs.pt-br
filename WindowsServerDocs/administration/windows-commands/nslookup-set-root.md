---
title: nslookup set root
description: Artigo de referência para o comando Set root do nslookup, que altera o nome do servidor raiz que é usado para consultas.
ms.topic: reference
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: d7e1251b7e13320a77a4c59736a6bd985bd248af
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89637487"
---
# <a name="nslookup-set-root"></a>nslookup set root

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o nome do servidor raiz usado para consultas.

> [!NOTE]
> Esse comando dá suporte ao comando [raiz nslookup](nslookup-root.md) .

## <a name="syntax"></a>Sintaxe

```
set root=<rootserver>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---------- | ---------- |
| `<rootserver>` | Especifica o novo nome para o servidor raiz. O valor padrão é **ns.NIC.DDN.mil**. |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [nslookup root](nslookup-root.md)
