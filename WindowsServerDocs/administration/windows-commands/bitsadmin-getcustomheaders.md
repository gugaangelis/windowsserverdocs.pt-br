---
title: bitsadmin getcustomheaders
description: Tópico de referência para o comando Bitsadmin getcustomheaders, que recupera os cabeçalhos HTTP personalizados do trabalho.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1f0d38d3-e865-4474-81e8-773d65c3d1cc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7fe839cd0e629af88b3ee3642abcce339442d03a
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718094"
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
