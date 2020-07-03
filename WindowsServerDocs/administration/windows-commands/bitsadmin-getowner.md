---
title: bitsadmin getowner
description: Artigo de referência para o comando Bitsadmin GetOwner, que recupera o proprietário do trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5203f84c-a879-4f31-ae3e-7ea74bd63ca5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f530952e510fdff1a30dfd7c6081e112d841f401
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85926897"
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
