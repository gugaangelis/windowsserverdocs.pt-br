---
title: bitsadmin setnoprogresstimeout
description: Artigo de referência para o comando Bitsadmin setnoprogresstimeout, que define o período de tempo, em segundos, que o serviço tenta transferir o arquivo depois que um erro transitório ocorre.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9bb7ac4cd03148c533aa27f62a1c9770465673a5
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927672"
---
# <a name="bitsadmin-setnoprogresstimeout"></a>bitsadmin setnoprogresstimeout

Define o período de tempo, em segundos, que o BITS tenta transferir o arquivo depois que o primeiro erro transitório ocorre.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setnoprogresstimeout <job> <timeoutvalue>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| tempo limite | O período de tempo que o BITS aguarda para transferir um arquivo após o primeiro erro, em segundos. |

### <a name="remarks"></a>Comentários

- O intervalo de tempo limite de "nenhum progresso" começa quando o trabalho encontra seu primeiro erro transitório.

- O intervalo de tempo limite é interrompido ou redefinido quando um byte de dados é transferido com êxito.

- Se o intervalo de tempo limite "no Progress" exceder o *TimeOutValue*, o trabalho será colocado em um estado de erro fatal.

## <a name="examples"></a>Exemplos

Para definir o valor de tempo limite "sem progresso" para 20 segundos, para o trabalho chamado *myDownloadJob*:

```
bitsadmin /setnoprogresstimeout myDownloadJob 20
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
