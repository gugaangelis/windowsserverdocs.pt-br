---
title: bitsadmin getbytestotal
description: Tópico de referência para o comando Bitsadmin getbytestotal, que recupera o tamanho do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 784e0bfa-7b09-4262-9104-adbc9beb479b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f844e1d3689c42a2c533921797d15dbb946b551e
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718158"
---
# <a name="bitsadmin-getbytestotal"></a>bitsadmin getbytestotal

Recupera o tamanho do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getbytestotal <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar o tamanho do trabalho chamado *myDownloadJob*:

```
bitsadmin /getbytestotal myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
