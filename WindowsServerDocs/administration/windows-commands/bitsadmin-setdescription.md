---
title: bitsadmin setdescription
description: Artigo de referência do comando Bitsadmin SetDescription, que define a descrição do trabalho especificado.
ms.topic: article
ms.assetid: 1e46a5dd-4637-4a2e-b88f-d3f85b177db8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d499b152f5cb3a846cc1de6ec65f07903421cf49
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87893173"
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
