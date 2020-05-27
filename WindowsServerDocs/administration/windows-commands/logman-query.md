---
title: logman query
description: Tópico de referência para o comando de consulta logman, que consulta o coletor de dados ou as propriedades do conjunto de coletores de dados.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2179604bdc581fe24fa4d702ca5e223dc11579be
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820556"
---
# <a name="logman-query"></a>logman query

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consulta as propriedades do coletor de dados ou do conjunto de coletores de dados.

## <a name="syntax"></a>Sintaxe

```
logman query [providers|Data Collector Set name] [options]
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| --------- | ----------- |
| -s`<computer name>` | Execute o comando no computador remoto especificado. |
| -configuração`<value>` | Especifica o arquivo de configurações que contém as opções de comando. |
| [-n]`<name>` | O nome do objeto de destino. |
| -ETS | Envia comandos para sessões de rastreamento de eventos diretamente sem salvar ou agendar. |
| /? | Exibe a ajuda contextual. |

### <a name="examples"></a>Exemplos

Para listar todos os conjuntos de coletores de dados configurados no sistema de destino, digite:

```
logman query
```

Para listar os coletores de dados contidos no conjunto de coletores de dados denominado *perf_log*, digite:

```
logman query perf_log
```

Para listar todos os provedores disponíveis de coletores de dados no sistema de destino, digite:

```
logman query providers
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [logman](logman.md)
