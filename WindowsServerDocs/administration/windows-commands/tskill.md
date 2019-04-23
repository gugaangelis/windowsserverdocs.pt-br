---
title: tskill
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 08986e6a-6900-4ece-85a1-8f73b14db1b3 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 59958481a7c832aca7bc25d7d4d3ebbf4e8ef80c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59835037"
---
# <a name="tskill"></a>tskill

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Finaliza um processo em execução em uma sessão em um servidor de Host de sessão de área de trabalho remota (Host de sessão rd).
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
tskill {<ProcessID> | <ProcessName>} [/server:<ServerName>] [/id:<SessionID> | /a] [/v]
```

## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|\<ProcessID >|Especifica a ID do processo que você deseja encerrar.|
|\<ProcessName >|Especifica o nome do processo que você deseja encerrar. Esse parâmetro pode incluir caracteres curinga.|
|/server:\<ServerName>|Especifica o servidor de terminal que contém o processo que você deseja encerrar. Se **/server** não for especificado, o servidor de Host de sessão de área de trabalho remota atual será usado.|
|/id:\<SessionID>|Encerra o processo que está em execução na sessão especificada.|
|/a|Encerra o processo que está em execução em todas as sessões.|
|/v|Exibe informações sobre as ações que está sendo executada.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   Você pode usar **tskill** para encerrar apenas os processos que pertencem a você, a menos que você seja um administrador. Os administradores têm acesso completo a todos **tskill** funções e podem encerrar os processos em execução em outras sessões de usuário.
-   Quando terminam de todos os processos em execução em uma sessão, a sessão também termina.
-   Se você usar o *ProcessName* e o **/server: * * * ServerName* parâmetros, você deve também especificar o **/id: * * * SessionID* ou o **/a** parâmetro.

## <a name="BKMK_examples"></a>Exemplos
-   Para encerrar o processo 6543, digite:
    ```
    tskill 6543
    ```
-   Para encerrar o processo "explorer" em execução na sessão 5, digite:
    ```
    tskill explorer /id:5
    ```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)
