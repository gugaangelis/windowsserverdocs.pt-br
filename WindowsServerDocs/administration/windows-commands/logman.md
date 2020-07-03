---
title: logman
description: Artigo de referência para o comando logman, que cria e gerencia a sessão de rastreamento de eventos e os logs de desempenho e dá suporte a muitas funções do monitor de desempenho da linha de comando.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 574a5203-5b3b-4759-a678-f26d00dde447
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 993c96fbbcccd1b2a48303cc5926f25fd7899c4d
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85934566"
---
# <a name="logman"></a>logman

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Cria e gerencia os logs de Sessão de Rastreamento de Eventos e de Desempenho e dá suporte a várias funções do Monitor de Desempenho na linha de comando.

## <a name="syntax"></a>Sintaxe

```
logman [create | query | start | stop | delete| update | import | export | /?] [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| [logman create](logman-create.md) | Cria um contador, rastreamento, coletor de dados de configuração ou API. |
| [logman query](logman-query.md) | Consulta as propriedades do coletor de dados. |
| [logman start &#124; stop](logman-start-stop.md) | Inicia ou interrompe a coleta de dados. |
| [logman delete](logman-delete.md) | Exclui um coletor de dados existente. |
| [logman update](logman-update.md) | Atualiza as propriedades de um coletor de dados existente. |
| [logman import &#124; export](logman-import-export.md) | Importa um conjunto de coletores de dados de um arquivo XML ou exporta um conjunto de coletores de dados para um arquivo XML. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)