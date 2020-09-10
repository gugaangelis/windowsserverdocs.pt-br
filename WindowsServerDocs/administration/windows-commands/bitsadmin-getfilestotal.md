---
title: bitsadmin getfilestotal
description: Artigo de referência para o comando Bitsadmin getfilestotal, que recupera o número de arquivos no trabalho especificado.
ms.topic: reference
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 73a543914f31a7b9e5b028caf273d7945a954fdd
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632100"
---
# <a name="bitsadmin-getfilestotal"></a>bitsadmin getfilestotal

Recupera o número de arquivos no trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getfilestotal <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o número de arquivos incluídos no trabalho chamado *myDownloadJob*:

```
bitsadmin /getfilestotal myDownloadJob
```

## <a name="see-also"></a>Consulte Também

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
