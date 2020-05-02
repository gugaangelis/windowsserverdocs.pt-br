---
title: bitsadmin getnoprogresstimeout
description: Tópico de referência para o comando Bitsadmin getnoprogresstimeout, que recupera o período de tempo, em segundos, que o serviço tentará transferir o arquivo após ocorrer um erro transitório.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9cd9b19b-cbb4-4352-8419-978080f016b6
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3ee0377bde8a438f23ca571bc9859deef92f18fb
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717821"
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
