---
title: Consulta
description: Tópico de referência para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 675c5128-f3cf-4e8f-8a3f-b29ab2a8b6de
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1dcea0fa4ea91de56e81c51bf9fe87ec7e3a49fa
ms.sourcegitcommit: 4894649cc47dfa535306cc334871f81155198f76
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/01/2020
ms.locfileid: "84254709"
---
# <a name="query"></a>Consulta

> Aplica-se a: Windows Server (canal semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre os servidores de processos, sessões e Host da Sessão da Área de Trabalho Remota (Host da Sessão RD).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte Novidades do [serviços de área de trabalho remota no Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11)) na biblioteca Microsoft docs Windows Server.

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

- [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
- [Referência aos comandos dos Serviços de Área de Trabalho Remota (Serviços de Terminal)](remote-desktop-services-terminal-services-command-reference.md)
