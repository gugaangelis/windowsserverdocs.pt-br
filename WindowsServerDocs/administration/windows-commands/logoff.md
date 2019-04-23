---
title: fazer logoff
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 939f09cc-de8c-436c-a05d-aca5f2a06371
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 75dd5fa5860101f80179d9c602b1d919e7caacbc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831517"
---
# <a name="logoff"></a>fazer logoff

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Faz logoff de um usuário de uma sessão em um servidor de Host de sessão de área de trabalho remota (Host de sessão rd) e exclui a sessão do servidor.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
logoff [<SessionName> | <SessionID>] [/server:<ServerName>] [/v]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|<SessionName>|Especifica o nome da sessão.|
|<SessionID>|Especifica a ID numérica que identifica a sessão para o servidor.|
|/server:<ServerName>|Especifica o servidor de Host de sessão de área de trabalho remota que contém a sessão cujo usuário você deseja fazer logoff. Se não for especificado, o servidor no qual você está ativo no momento será usado.|
|/v|Exibe informações sobre as ações que está sendo executada.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
-   Você pode sempre fazer logoff da sessão para o qual você fez logon. No entanto, você deve ter permissão de controle total para fazer logoff de usuários de outras sessões.
-   O logoff de um usuário de uma sessão sem aviso pode resultar em perda de dados na sessão do usuário. Você deve enviar uma mensagem para o usuário usando o **msg** comando para avisar o usuário antes de realizar esta ação.
-   Se <*SessionID*> ou <*SessionName*> não for especificado, **logoff** desconecta o usuário da sessão atual. Se você especificar <*SessionName*>, ele deve ser um Active Directory.
-   Ao fazer logoff de um usuário, todos os processos são encerrados e a sessão é excluído do servidor.
-   Você não pode fazer logoff de um usuário da sessão de console.
## <a name="BKMK_examples"></a>Exemplos
-   Para fazer logoff de um usuário da sessão atual, digite:
    ```
    logoff
    ```
-   Para fazer logoff de um usuário de uma sessão usando a identificação da sessão, por exemplo sessão 12, digite:
    ```
    logoff 12
    ```
-   Para fazer logoff de um usuário de uma sessão usando o nome da sessão e servidor, por exemplo, sessão TERM04 no servidor1, digite:
    ```
    logoff TERM04 /server:Server1
    ```
    
#### <a name="additional-references"></a>Referências adicionais
-   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
-   [Serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)
