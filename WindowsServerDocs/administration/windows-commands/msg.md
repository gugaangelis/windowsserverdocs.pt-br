---
title: msg
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9501cf3e-568e-4982-9987-8daecc6c17ff
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 68a393b57a255915b93759b4b26286ce4d838019
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66437226"
---
# <a name="msg"></a>msg

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia uma mensagem a um usuário em um servidor de Host de sessão de área de trabalho remota (Host de sessão rd).
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

## <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                                                               Descrição                                                                                                                               |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      <UserName>      |                                                                                                  Especifica o nome do usuário que você deseja receber a mensagem.                                                                                                   |
|    <SessionName>     |                                                                                                 Especifica o nome da sessão que você deseja receber a mensagem.                                                                                                 |
|     <SessionID>      |                                                                                            Especifica a ID numérica da sessão cujo usuário você deseja receber uma mensagem.                                                                                            |
|     @<FileName>      |                                                                         Identifica um arquivo que contém uma lista de nomes de usuário, nomes de sessão e as identificações de sessão que você deseja receber a mensagem.                                                                         |
|          \*          |                                                                                                           Envia a mensagem a todos os nomes de usuário no sistema.                                                                                                            |
| /server:<ServerName> |                                              Especifica o servidor de Host de sessão de área de trabalho remota, cuja sessão ou usuário que você deseja receber a mensagem. Se não for especificado, **/server** usa o servidor ao qual você fez logon.                                              |
|   /time:<Seconds>    | Especifica a quantidade de tempo que a mensagem enviada é exibida na tela do usuário. Depois que o tempo limite for atingido, a mensagem desaparecerá. Se nenhum limite de tempo for definido, a mensagem permanecerá na tela do usuário até que o usuário vê a mensagem e clica **Okey**. |
|          /v          |                                                                                                         Exibe informações sobre as ações que está sendo executada.                                                                                                         |
|          /w          |         Aguarda uma confirmação do usuário que a mensagem foi recebida. Use este parâmetro com **/hora:** <*segundos*> para evitar um possível grande atraso se o usuário não responder imediatamente. O uso desse parâmetro com **/v** também é útil.          |
|      <Message>       |                  Especifica o texto da mensagem que você deseja enviar. Se nenhuma mensagem for especificada, você será solicitado a inserir uma mensagem. Para enviar uma mensagem que está contida em um arquivo, digite o símbolo de menor que (<) seguido pelo nome do arquivo.                  |
|          /?          |                                                                                                                  Exibe a ajuda no prompt de comando.                                                                                                                   |

## <a name="remarks"></a>Comentários
-   Se você não especificar um usuário ou uma sessão, **msg** exibe uma mensagem de erro. Ao especificar uma sessão, ele deve ser um Active Directory.
-   O usuário deve ter permissão de acesso especial de mensagem para enviar uma mensagem.

## <a name="BKMK_examples"></a>Exemplos
-   Para enviar que a mensagem intitulada "Vamos nos encontrar às 13:30 hoje" para todas as sessões para o Usuário1, digite:
    ```
    msg User1 Let's meet at 1PM today
    ```
-   Para enviar a mesma mensagem para a sessão modeM02, digite:
    ```
    msg modem02 Let's meet at 1PM today
    ```
-   Para enviar a mensagem para a sessão 12, digite:
    ```
    msg 12 Let's meet at 1PM today
    ```
-   Para enviar a mensagem para todas as sessões contidas no arquivo USERlist, digite:
    ```
    msg @userlist Let's meet at 1PM today
    ```
-   Para enviar a mensagem a todos os usuários que fizerem logon, digite:
    ```
    msg * Let's meet at 1PM today
    ```
-   Para enviar a mensagem a todos os usuários, com um tempo limite de confirmação (por exemplo, 10 segundos), digite:
    ```
    msg * /time:10 Let's meet at 1PM today
    ```

#### <a name="additional-references"></a>Referências adicionais
-  [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-  [Serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)
