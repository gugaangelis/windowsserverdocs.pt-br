---
title: bitsadmin wrap
description: Artigo de referência para o comando Bitsadmin Wrap, que encapsula qualquer linha de texto de saída que se estende além da borda mais à direita da janela de comando para a próxima linha.
ms.topic: reference
ms.assetid: 14e57522-539d-4621-ad15-09f7a44ccab7
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 24fa50e153d52ec74ab728a132aadd1b95c7a9da
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630386"
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
