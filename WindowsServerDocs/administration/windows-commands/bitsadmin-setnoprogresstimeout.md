---
title: bitsadmin setnoprogresstimeout
description: Artigo de referência para o comando Bitsadmin setnoprogresstimeout, que define o período de tempo, em segundos, que o serviço tenta transferir o arquivo depois que um erro transitório ocorre.
ms.topic: reference
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: dc2559a963900234fd3111edb1a32e3f13b27e40
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89027794"
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
