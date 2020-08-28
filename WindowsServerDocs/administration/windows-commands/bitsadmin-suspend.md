---
title: bitsadmin suspend
description: Artigo de referência para o comando Bitsadmin Suspend, que suspende o trabalho especificado.
ms.topic: reference
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ad4ca877ac9e5548f3d3ec830f8fc2822f781218
ms.sourcegitcommit: 96d46c702e7a9c3a321bbbb5284f73911c7baa3c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/27/2020
ms.locfileid: "89033384"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Suspende o trabalho especificado. Se você suspendeu o trabalho por engano, poderá usar a opção [Bitsadmin resume](bitsadmin-resume.md) para reiniciar o trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ---------- |
| trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="example"></a>Exemplo

Para suspender o trabalho chamado *myDownloadJob*:


```
bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando retomar Bitsadmin](bitsadmin-resume.md)

- [comando Bitsadmin](bitsadmin.md)
