---
title: bitsadmin getdescription
description: Artigo de referência para o comando GetDescription do Bitsadmin, que recupera a descrição do trabalho especificado.
ms.topic: reference
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c8233ab420cadf3e7f578ce6f38d7631a8a1e820
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89030414"
---
# <a name="bitsadmin-getdescription"></a>bitsadmin getdescription

Recupera a descrição do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getdescription <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar a descrição para o trabalho chamado *myDownloadJob*:

```
bitsadmin /getdescription myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
