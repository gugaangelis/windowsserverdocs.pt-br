---
title: bitsadmin setminretrydelay
description: Artigo de referência para o comando Bitsadmin setminretrydelay, que define o período mínimo de tempo, em segundos, que o BITS aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.
ms.topic: article
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 448e03805f30af50abffa28365e456028598ae1a
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893080"
---
# <a name="bitsadmin-setminretrydelay"></a>bitsadmin setminretrydelay

Define o período mínimo de tempo, em segundos, que o BITS aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setminretrydelay <job> <retrydelay>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| retrydelay | Período mínimo de tempo para que os BITS aguardem após um erro durante a transferência, em segundos. |

## <a name="examples"></a>Exemplos

Para definir o atraso mínimo de repetição para 35 segundos para o trabalho chamado *myDownloadJob*:

```
bitsadmin /setminretrydelay myDownloadJob 35
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
