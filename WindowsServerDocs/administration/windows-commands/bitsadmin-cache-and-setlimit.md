---
title: Bitsadmin cache e setlimit
description: Tópico de comandos do Windows para **cache Bitsadmin e setlimit**, que define o limite de tamanho do cache.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 46578835-d5ce-423b-be4d-62ddb9e1908d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 746ee0b69da8f5bd22fec2ccbd432126cc25d94d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850869"
---
# <a name="bitsadmin-cache-and-setlimit"></a>Bitsadmin cache e setlimit

Define o limite de tamanho do cache.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /cache /setlimit percent
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| {1&gt;percent&lt;1} | O limite de cache definido como uma porcentagem do espaço total no disco rígido. |

## <a name="examples"></a><a name=BKMK_examples></a>Disso

O exemplo a seguir limita o tamanho do cache para 50%.

```
C:\>bitsadmin /cache /setlimit 50
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)