---
title: bitsadmin getproxybypasslist
description: Tópico de referência para o comando Bitsadmin getproxybypasslist, que recupera a lista de bypass de proxy para o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3b4f37d09521c28d55104975ed754a10e8df011e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717669"
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
