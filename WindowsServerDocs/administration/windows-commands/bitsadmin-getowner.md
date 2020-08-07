---
title: bitsadmin getowner
description: Artigo de referência para o comando Bitsadmin GetOwner, que recupera o proprietário do trabalho especificado.
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91dd63dcba8b41fee01c17e87c817e32a9dbba39
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87894045"
---
# <a name="bitsadmin-getowner"></a>bitsadmin getowner

Exibe o nome de exibição ou o GUID do proprietário do trabalho especificado.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /getowner <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| -------------- | -------------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="examples"></a>Exemplos

Para exibir o proprietário do trabalho chamado *myDownloadJob*:

```
bitsadmin /getowner myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
