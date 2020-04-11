---
title: bitsadmin setnoprogresstimeout
description: O tópico de comandos do Windows para **Bitsadmin setnoprogresstimeout**, que define o período de tempo, em segundos, que o serviço tenta transferir o arquivo após ocorrer um erro transitório.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7fac50d9-cc6b-46a4-a96f-fab751ee1756
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8adff95b0dbae68634db2e248d4493549c5ac85d
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81122873"
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

## <a name="remarks"></a>Comentários

- O intervalo de tempo limite de "nenhum progresso" começa quando o trabalho encontra seu primeiro erro transitório.

- O intervalo de tempo limite é interrompido ou redefinido quando um byte de dados é transferido com êxito.

- Se o intervalo de tempo limite "no Progress" exceder o *TimeOutValue*, o trabalho será colocado em um estado de erro fatal.

## <a name="examples"></a>Exemplos

O exemplo a seguir define o valor de tempo limite "no Progress" como 20 segundos para o trabalho chamado *myDownloadJob*.

```
C:\>bitsadmin /setnoprogresstimeout myDownloadJob 20
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)