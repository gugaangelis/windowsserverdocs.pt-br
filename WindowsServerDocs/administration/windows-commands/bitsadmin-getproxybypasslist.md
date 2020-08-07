---
title: bitsadmin getproxybypasslist
description: Artigo de referência para o comando Bitsadmin getproxybypasslist, que recupera a lista de bypass de proxy para o trabalho especificado.
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 16cca14a47f086be65764da5441d915d2d28d2db
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894008"
---
# <a name="bitsadmin-getproxybypasslist"></a>bitsadmin getproxybypasslist

Recupera a lista de bypass de proxy para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getproxybypasslist <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

### <a name="remarks"></a>Comentários

A lista de bypass contém os nomes de host ou endereços IP, ou ambos, que não devem ser roteados por meio de um proxy. A lista pode conter `<local>` para se referir a todos os servidores na mesma LAN. A lista pode ser ponto e vírgula (;) ou delimitado por espaço.

## <a name="examples"></a>Exemplos

Para recuperar a lista de bypass de proxy para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getproxybypasslist myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
