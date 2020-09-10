---
title: bitsadmin resume
description: Artigo de referência do comando Bitsadmin resume, que ativa um trabalho novo ou suspenso na fila de transferência.
ms.topic: reference
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 2ccab242034af3e0b5e01f0efec60d5309879516
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631101"
---
# <a name="bitsadmin-resume"></a>bitsadmin resume

Ativa um trabalho novo ou suspenso na fila de transferência. Se você retomou seu trabalho por engano ou simplesmente precisar suspender seu trabalho, poderá usar a opção de [suspensão Bitsadmin](bitsadmin-suspend.md) para suspender o trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /resume <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para retomar o trabalho chamado *myDownloadJob*:

```
bitsadmin /resume myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando de suspensão Bitsadmin](bitsadmin-suspend.md)

- [comando Bitsadmin](bitsadmin.md)
