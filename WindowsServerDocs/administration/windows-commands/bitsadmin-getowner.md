---
title: bitsadmin getowner
description: Tópico de referência para o comando Bitsadmin GetOwner, que recupera o proprietário do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2a345ea47232f1f4d6340e1341747c9dad92382b
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82717714"
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
