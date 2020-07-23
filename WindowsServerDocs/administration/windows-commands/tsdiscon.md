---
title: tsdiscon
description: Artigo de referência para tsdiscon, que desconecta uma sessão de um servidor de host de sessão de área de trabalho remota.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3016bb5a146bf7c1d55ab93325debb11b0f6a81c
ms.sourcegitcommit: d5e27c1f2f168a71ae272bebf8f50e1b3ccbcca3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "86954928"
---
# <a name="tsdiscon"></a>tsdiscon

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desconecta uma sessão de um servidor Host da Sessão da Área de Trabalho Remota.



> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](/previous-versions/orphan-topics/ws.11/hh831527(v=ws.11)) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|\<SessionId>|Especifica a ID da sessão a ser desconectada.|
|\<SessionName>|Especifica o nome da sessão a ser desconectada.|
|/server:\<ServerName>|Especifica o servidor de terminal que contém a sessão que você deseja desconectar. Caso contrário, o servidor host da Sessão RD atual será usado.|
|/v|Exibe informações sobre as ações que estão sendo executadas.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   Você deve ter permissão controle total ou desconectar permissão de acesso especial para desconectar outro usuário de uma sessão.
-   Se nenhum ID de sessão ou nome de sessão for especificado, **tsdiscon** desconectará a sessão atual.
-   Todos os aplicativos que estavam em execução quando você desconectou a sessão são executados automaticamente quando você se reconectar a essa sessão sem perda de dados. Use **Redefinir sessão** para encerrar os aplicativos em execução da sessão desconectada, mas lembre-se de que isso pode resultar na perda de dados na sessão.
-   O parâmetro **/Server** será necessário somente se você usar o **tsdiscon** de um servidor remoto.
-   A sessão do console não pode ser desconectada.

## <a name="examples"></a>Exemplos
- Para desconectar a sessão atual, digite:
  ```
  tsdiscon
  ```
- Para desconectar a sessão 10, digite:
  ```
  tsdiscon 10
  ```
- Para desconectar a sessão chamada TERM04, digite:
  ```
  tsdiscon TERM04
  ```
  ## <a name="additional-references"></a>Referências adicionais
  - Chave de sintaxe [de linha de comando](command-line-syntax-key.md) 
   [Referência de comando de serviços de área de trabalho remota (serviços de terminal)](remote-desktop-services-terminal-services-command-reference.md)
