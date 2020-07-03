---
title: pushprinterconnections
description: Artigo de referência para o comando PushPrinterConnections, que lê as configurações de conexão de impressora implantadas de Política de Grupo e implanta/remove conexões de impressora conforme necessário.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7b22fd5143a9477b40a515df44c9a0b5663dfd7a
ms.sourcegitcommit: 2afed2461574a3f53f84fc9ec28d86df3b335685
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85931378"
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
