---
title: pushprinterconnections
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55294b27ec51edb7083debf15e332117b9b4e1df
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821226"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



Lê as configurações de conexão de impressora implantadas de Política de Grupo e implanta/remove conexões de impressora conforme necessário.

## <a name="syntax"></a>Sintaxe

```
pushprinterconnections <-log> <-?>
```

#### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|---------|-----------|
|> de < log|Grava um arquivo de log de depuração por usuário em% temp ou grava um log de depuração por computador em%WINDIR%\temp.|
|<-? >|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Esse utilitário é para uso na inicialização da máquina ou nos scripts de logon do usuário e não deve ser executado na linha de comando.

## <a name="additional-references"></a>Referências adicionais

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Implantar impressoras usando o Política de Grupo](https://go.microsoft.com/fwlink/?LinkId=230627)