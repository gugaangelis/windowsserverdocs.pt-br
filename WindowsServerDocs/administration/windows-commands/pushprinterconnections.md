---
title: pushprinterconnections
description: Tópico de referência para o comando PushPrinterConnections, que lê as configurações de conexão de impressora implantadas de Política de Grupo e implanta/remove conexões de impressora conforme necessário.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 701089f2597b1d4e7bc05f7949dbc80dee3535bb
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85472121"
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
