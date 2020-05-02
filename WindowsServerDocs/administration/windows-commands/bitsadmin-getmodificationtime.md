---
title: bitsadmin getmodificationtime
description: Tópico de referência para o comando Bitsadmin getmodificatime, que recupera a última vez que o trabalho foi modificado ou os dados foram transferidos com êxito.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bab8c317917894a351c03df1efefb17842ecb7d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717835"
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
