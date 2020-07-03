---
title: bitsadmin setdescription
description: Artigo de referência do comando Bitsadmin SetDescription, que define a descrição do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9c22909193667ff3222576010498d7b2fbcf14c7
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85927837"
---
# <a name="bitsadmin-setdescription"></a>bitsadmin setdescription

Define a descrição para o trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /setdescription <job> <description>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |
| descrição | Texto usado para descrever o trabalho. |

## <a name="examples"></a>Exemplos

Para recuperar a descrição para o trabalho chamado *myDownloadJob*:

```
bitsadmin /setdescription myDownloadJob music_downloads
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
