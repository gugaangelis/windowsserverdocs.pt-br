---
title: Bitsadmin getproxylist – recupera a lista de proxy para o trabalho especificado.
description: Artigo de referência para o comando Bitsadmin getproxylist, que recupera a lista de proxy para o trabalho especificado.
ms.topic: article
ms.assetid: eebfa727-d8f1-4ae3-9382-6d8ffe8c3df3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bcd94c65a11006a795f071224397d8b3081b7548
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893971"
---
# <a name="bitsadmin-getproxylist"></a>bitsadmin getproxylist

Recupera a lista delimitada por vírgulas de servidores proxy a serem usados para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getproxylist <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar a lista de proxy para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getproxylist myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
