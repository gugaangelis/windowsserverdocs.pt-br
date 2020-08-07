---
title: logman query
description: Artigo de referência para o comando de consulta logman, que consulta o coletor de dados ou as propriedades do conjunto de coletores de dados.
ms.topic: article
ms.assetid: 1116a0f0-5415-4369-a045-12f79f8f66de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2cb324651001f071e45acf0821f402458ed838d8
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87887282"
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

- [comando logman](logman.md)
