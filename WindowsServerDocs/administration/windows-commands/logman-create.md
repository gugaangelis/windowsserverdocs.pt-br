---
title: logman create
description: Artigo de referência para o comando logman Create, que cria um contador, rastreamento, coletor de dados de configuração ou API.
ms.topic: reference
ms.assetid: 972f0126-7bc4-4b14-9265-062864f3ffd4
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 1115591f097c81b9b23dedbf3739c10346d4932e
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89640375"
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