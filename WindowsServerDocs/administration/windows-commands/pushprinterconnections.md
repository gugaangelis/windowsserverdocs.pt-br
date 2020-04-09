---
title: pushprinterconnections
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5941b1eba55ce7524946f3257c093d409ef7d773
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837069"
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
|<-? >|Exibe a Ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários

Esse utilitário é para uso na inicialização da máquina ou nos scripts de logon do usuário e não deve ser executado na linha de comando.

## <a name="additional-references"></a>Referências adicionais

-   - [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-   [Implantar impressoras usando o Política de Grupo](https://go.microsoft.com/fwlink/?LinkId=230627)