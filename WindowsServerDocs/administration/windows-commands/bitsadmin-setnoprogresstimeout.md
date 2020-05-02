---
title: bitsadmin setnoprogresstimeout
description: Tópico de referência para o comando Bitsadmin setnoprogresstimeout, que define o período de tempo, em segundos, que o serviço tenta transferir o arquivo após ocorrer um erro transitório.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 398882cf795e98dc0bbc0fb81006d3406fded707
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720114"
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
