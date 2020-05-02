---
title: bitsadmin getdescription
description: Tópico de referência para o comando GetDescription do Bitsadmin, que recupera a descrição do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f3974603-ebbe-4d31-8217-040fe2d90c85
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ec5fa9875ca9f669c2a43d58532d3e5e0770d550
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718076"
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
