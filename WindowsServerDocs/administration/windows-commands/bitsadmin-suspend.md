---
title: bitsadmin suspend
description: O tópico de comandos do Windows para **Bitsadmin Suspend**, que suspende o trabalho especificado.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 42ed83d4dbf8c3d982c5c186b440cf17997903c9
ms.sourcegitcommit: 141f2d83f70cb467eee59191197cdb9446d8ef31
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123151"
---
# <a name="bitsadmin-suspend"></a>bitsadmin suspend

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Suspende o trabalho especificado. Se você suspendeu o trabalho por engano, poderá usar a opção [Bitsadmin resume](bitsadmin-resume.md) para reiniciar o trabalho.

## <a name="syntax"></a>Sintaxe

```
bitsadmin /suspend <job>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ---------- |
| Trabalho | O nome de exibição ou o GUID do trabalho. |

## <a name="example"></a>{1&gt;Exemplo&lt;1}

O exemplo a seguir suspende o trabalho chamado *myDownloadJob*.


```
C:\>bitsadmin /suspend myDownloadJob
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
