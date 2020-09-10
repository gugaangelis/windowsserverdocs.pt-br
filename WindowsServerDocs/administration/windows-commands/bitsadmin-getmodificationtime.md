---
title: bitsadmin getmodificationtime
description: Artigo de referência para o comando Bitsadmin getmodificatime, que recupera a última vez que o trabalho foi modificado ou os dados foram transferidos com êxito.
ms.topic: reference
ms.assetid: e543945e-92c4-491e-8c2d-344f8a3e342d
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 293b8ef47374c0df3f9b1cd3d76802d592c3c522
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89631932"
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
