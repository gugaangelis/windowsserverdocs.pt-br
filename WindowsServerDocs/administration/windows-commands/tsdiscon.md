---
title: tsdiscon
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 13139674-7dee-4965-8cac-32f4928e8b9a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a1b5fca329864ebed9eab66671a17493f0fc3ca8
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440917"
---
# <a name="tsdiscon"></a>tsdiscon

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Desconecta uma sessão de um servidor de Host de sessão de área de trabalho remota (Host de sessão rd).
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
tsdiscon [<SessionID> | <SessionName>] [/server:<ServerName>] [/v]
```

## <a name="parameters"></a>Parâmetros

|Parâmetro|Descrição|
|-------|--------|
|\<SessionId>|Especifica a ID de sessão a ser desconectada.|
|\<SessionName>|Especifica o nome da sessão a ser desconectada.|
|/server:\<ServerName>|Especifica o servidor de terminal que contém a sessão que você deseja desconectar. Caso contrário, o servidor de Host de sessão de área de trabalho remota atual é usado.|
|/v|Exibe informações sobre as ações que está sendo executada.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   Você deve ter a permissão Controle total ou desconectar-se a permissão de acesso especial para desconectar outro usuário de uma sessão.
-   Se nenhuma ID de sessão ou o nome da sessão for especificado, **tsdiscon** desconecta a sessão atual.
-   Todos os aplicativos que estavam em execução quando a sessão é desconectada automaticamente em execução quando você se reconectar à sessão sem perda de dados. Use **redefinir sessão** para encerrar os aplicativos em execução da sessão desconectada, mas lembre-se que isso pode resultar em perda de dados da sessão.
-   O **/server** parâmetro é necessário apenas se você usar **tsdiscon** de um servidor remoto.
-   A sessão de console não poderá ser desconectada.

## <a name="BKMK_examples"></a>Exemplos
- Para desconectar a sessão atual, digite:
  ```
  tsdiscon
  ```
- Para desconectar a sessão 10, digite:
  ```
  tsdiscon 10
  ```
- Para desconectar a sessão denominada TERM04, digite:
  ```
  tsdiscon TERM04
  ```
  #### <a name="additional-references"></a>Referências adicionais
  [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
  [serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)
