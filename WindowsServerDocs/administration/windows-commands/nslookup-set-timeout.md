---
title: nslookup set timeout
description: Tópico de referência para o comando set timeout do nslookup, que altera o número inicial de segundos para aguardar uma resposta a uma solicitação de pesquisa.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0d8fd0d96226e193ba723cc0a726ddf5362a538c
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721381"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Altera o número inicial de segundos para aguardar uma resposta a uma solicitação de pesquisa. Se uma resposta não for recebida dentro do período de tempo especificado, o período de tempo limite será duplicado e a solicitação será reenviada. Use o comando [set Retry do nslookup](nslookup-set-retry.md) para determinar o número de vezes para tentar enviar a solicitação.

## <a name="syntax"></a>Sintaxe

```
set timeout=<number>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
| ---------- | ---------- |
| `<number>` | Especifica o número de segundos de espera por uma resposta. O número padrão de segundos a aguardar é **5**. |
| /? | Exibe a ajuda no prompt de comando. |
| /help | Exibe a ajuda no prompt de comando. |

### <a name="examples"></a>Exemplos

Para definir o tempo limite para obter uma resposta para 2 segundos:

```
set timeout=2
```

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [nslookup set retry](nslookup-set-retry.md)
