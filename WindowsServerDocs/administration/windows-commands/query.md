---
title: query
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 675c5128-f3cf-4e8f-8a3f-b29ab2a8b6de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8d89ae8c7c526bce396b2583abc1728456f7bcc3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836819"
---
# <a name="query"></a>query

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre os servidores de processos, sessões e Host da Sessão da Área de Trabalho Remota (Host da Sessão RD).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
query process
query session
query termserver
query user
```

### <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|[processo de consulta](query-process.md)|Exibe informações sobre os processos que estão sendo executados em um servidor de host de sessão de área de trabalho remota.|
|[sessão de consulta](query-session.md)|Exibe informações sobre sessões em um servidor de host de sessão de área de trabalho remota.|
|[termserver de consulta](query-termserver.md)|Exibe uma lista de todos os servidores de host da sessão da área de trabalho remota na rede.|
|[consultar usuário](query-user.md)|Exibe informações sobre sessões de usuário em um servidor de host de sessão de área de trabalho remota.|

## <a name="additional-references"></a>Referências adicionais
- [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[referência de comando serviços de área de trabalho remota (serviços de terminal)](remote-desktop-services-terminal-services-command-reference.md)
