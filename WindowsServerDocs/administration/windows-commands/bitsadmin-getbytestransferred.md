---
title: bitsadmin getbytestransferred
description: Tópico de comandos do Windows para **Bitsadmin getbytestransferred**, que recupera o número de bytes transferidos para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 47bbf184-e06f-4be0-b2ba-d32b10d82002
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 957b3e60bf8a5e41b3964f4d762633472606654d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850769"
---
# <a name="bitsadmin-getbytestransferred"></a>bitsadmin getbytestransferred

Recupera o número de bytes transferidos para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getbytestransferred <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir recupera o número de bytes transferidos para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /getbytestransferred myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)