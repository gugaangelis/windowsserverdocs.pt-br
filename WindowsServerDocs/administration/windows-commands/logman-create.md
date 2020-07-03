---
title: logman create
description: Artigo de referência para o comando logman Create, que cria um contador, rastreamento, coletor de dados de configuração ou API.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 972f0126-7bc4-4b14-9265-062864f3ffd4
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 695a101a0aa6a720b64ffee6617085d13b6e83d1
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85922329"
---
# <a name="logman-create"></a>logman create

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria um contador, rastreamento, coletor de dados de configuração ou API.

## <a name="syntax"></a>Sintaxe

```
logman create <counter | trace | alert | cfg | api> <[-n] <name>> [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| [logman create counter](logman-create-counter.md) | Cria um coletor de dados de contador. |
| [logman create trace](logman-create-trace.md) | Cria um coletor de dados de rastreamento. |
| [logman create alert](logman-create-alert.md) | Cria um coletor de dados de alerta. |
| [logman create cfg](logman-create-cfg.md) | Cria um coletor de dados de configuração. |
| [logman create api](logman-create-api.md) | Cria um coletor de dados de rastreamento de API. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [comando logman](logman.md)