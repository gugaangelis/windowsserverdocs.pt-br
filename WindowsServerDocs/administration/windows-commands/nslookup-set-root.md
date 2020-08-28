---
title: nslookup set root
description: Artigo de referência para o comando Set root do nslookup, que altera o nome do servidor raiz que é usado para consultas.
ms.topic: reference
ms.assetid: 8ad5393c-d4fd-4594-8187-576b1dcde60a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2b59f54445266bd1b4c12b04cf8011fc6a2321b9
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89025160"
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
