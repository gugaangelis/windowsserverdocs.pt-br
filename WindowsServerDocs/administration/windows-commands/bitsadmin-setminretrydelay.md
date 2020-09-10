---
title: bitsadmin setminretrydelay
description: Artigo de referência para o comando Bitsadmin setminretrydelay, que define o período mínimo de tempo, em segundos, que o BITS aguarda depois de encontrar um erro transitório antes de tentar transferir o arquivo.
ms.topic: reference
ms.assetid: ce8674ca-6cc5-4bb2-8dda-7dfbb1cd6830
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 7ddce1aea607ed279aaa6c582ddc1661d269204e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630828"
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
