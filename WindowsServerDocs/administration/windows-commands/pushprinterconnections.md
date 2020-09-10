---
title: pushprinterconnections
description: Artigo de referência para o comando PushPrinterConnections, que lê as configurações de conexão de impressora implantadas de Política de Grupo e implanta/remove conexões de impressora conforme necessário.
ms.topic: reference
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: ef1cfc110d446da461251b9c7e28a4595edee291
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639935"
---
# <a name="pushprinterconnections"></a>pushprinterconnections

Lê as configurações de conexão de impressora implantadas de Política de Grupo e implanta/remove conexões de impressora conforme necessário.

> [!IMPORTANT]
> Esse utilitário é para uso na inicialização do computador ou em scripts de logon do usuário e não deve ser executado na linha de comando.

## <a name="syntax"></a>Sintaxe

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>Parâmetros

| Parâmetro | Descrição |
|--|--|
| > de < log | Grava um arquivo de log de depuração por usuário em *% Temp*ou grava um log de depuração por computador em *%WINDIR%\temp*. |
| <-? > | Exibe a ajuda no prompt de comando. |

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)

- [Referência aos comandos de impressão](print-command-reference.md)

- [Implantar impressoras usando o Política de Grupo](https://go.microsoft.com/fwlink/?LinkId=230627)
