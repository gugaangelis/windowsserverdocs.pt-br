---
title: bitsadmin getcustomheaders
description: Artigo de referência para o comando Bitsadmin getcustomheaders, que recupera os cabeçalhos HTTP personalizados do trabalho.
ms.topic: reference
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ec061c9e7f195d463525031bcbefc97913d69083
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89632190"
---
# <a name="bitsadmin-getcustomheaders"></a>bitsadmin getcustomheaders

Recupera os cabeçalhos HTTP personalizados do trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getcustomheaders <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para obter os cabeçalhos personalizados para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getcustomheaders myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
