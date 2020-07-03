---
title: bitsadmin resume
description: Artigo de referência do comando Bitsadmin resume, que ativa um trabalho novo ou suspenso na fila de transferência.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7c7540a9-a11a-4910-923a-2a2a61cbf11d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cd2a8dcb486c584ef4adaf96a5288a9db32d4553
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927960"
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
