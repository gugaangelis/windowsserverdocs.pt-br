---
title: bitsadmin getnoprogresstimeout
description: Artigo de referência para o comando Bitsadmin getnoprogresstimeout, que recupera o período de tempo, em segundos, que o serviço tentará transferir o arquivo após ocorrer um erro transitório.
ms.topic: reference
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51a8327669bdadbebec28329e0c6b11cc32e672b
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033514"
---
# <a name="bitsadmin-getnoprogresstimeout"></a>bitsadmin getnoprogresstimeout

Recupera o período de tempo, em segundos, que o serviço tentará transferir o arquivo depois que ocorrer um erro transitório.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getnoprogresstimeout <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o valor de tempo limite de progresso para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getnoprogresstimeout myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
