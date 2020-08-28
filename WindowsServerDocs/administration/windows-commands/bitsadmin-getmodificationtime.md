---
title: bitsadmin getmodificationtime
description: Artigo de referência para o comando Bitsadmin getmodificatime, que recupera a última vez que o trabalho foi modificado ou os dados foram transferidos com êxito.
ms.topic: reference
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 763ca0f60fa834ad14b92eb7963107fa407a8964
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89028724"
---
# <a name="bitsadmin-getmodificationtime"></a>bitsadmin getmodificationtime

Recupera a última vez que o trabalho foi modificado ou os dados foram transferidos com êxito.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getmodificationtime <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar a hora da última modificação para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getmodificationtime myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
