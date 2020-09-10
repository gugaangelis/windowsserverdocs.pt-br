---
title: bitsadmin getproxybypasslist
description: Artigo de referência para o comando Bitsadmin getproxybypasslist, que recupera a lista de bypass de proxy para o trabalho especificado.
ms.topic: reference
ms.assetid: 50959be3-7014-4bc9-9a7b-68f1ff94a94a
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 5b9b9ffd3865ef70408c566bdd832005e74f6598
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631753"
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
