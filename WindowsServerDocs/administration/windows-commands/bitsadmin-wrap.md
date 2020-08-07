---
title: bitsadmin wrap
description: Artigo de referência para o comando Bitsadmin Wrap, que encapsula qualquer linha de texto de saída que se estende além da borda mais à direita da janela de comando para a próxima linha.
ms.topic: article
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5d17678ec735f9e7d6319368b0b35a67b47ea576
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87880762"
---
# <a name="bitsadmin-wrap"></a>bitsadmin wrap

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Encapsula qualquer linha de texto de saída que se estenda além da borda mais à direita da janela de comando para a próxima linha. Você deve especificar essa opção antes de qualquer outra opção.

Por padrão, todas as opções, exceto a opção de [Monitor Bitsadmin](bitsadmin-monitor.md) , encapsulam o texto de saída.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /wrap <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ---------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar informações para o trabalho chamado *myDownloadJob* e encapsular o texto de saída:

```
bitsadmin /wrap /info myDownloadJob /verbose
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
