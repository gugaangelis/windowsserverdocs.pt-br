---
title: bitsadmin getfilestotal
description: Artigo de referência para o comando Bitsadmin getfilestotal, que recupera o número de arquivos no trabalho especificado.
ms.topic: reference
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 74e5dad863b12b7f90ed74bca0e6b0b352fb1360
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033604"
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
