---
title: bitsadmin suspend
description: Artigo de referência para o comando Bitsadmin Suspend, que suspende o trabalho especificado.
ms.topic: reference
ms.assetid: f9d42500-7bea-4aa8-a9f0-c22f6ed3e73b
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 0827f7fe42a41d981c165e41b1a8af71ec5d7472
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89630589"
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
