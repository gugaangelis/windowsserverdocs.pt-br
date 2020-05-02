---
title: bitsadmin getfilestotal
description: Tópico de referência para o comando Bitsadmin getfilestotal, que recupera o número de arquivos no trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c5de113e-f29c-4cd3-9392-0e300018d516
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5cf3b33c15bb18c8a141408f82fdd72a6e710817
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717973"
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
