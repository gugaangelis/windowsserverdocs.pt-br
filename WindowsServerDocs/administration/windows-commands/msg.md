---
title: msg
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 42a614f313d1e68dbf78d19a498563b541c52be1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373428"
---
# <a name="msg"></a>msg

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Envia uma mensagem a um usuário em um servidor de Host da Sessão da Área de Trabalho Remota (host de sessão da área de trabalho remota).
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
msg {<UserName> | <SessionName> | <SessionID>| @<FileName> | *} [/server:<ServerName>] [/time:<Seconds>] [/v] [/w] [<Message>]
```

## <a name="parameters"></a>Parâmetros

|      Parâmetro       |                                                                                                                               Descrição                                                                                                                               |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|      <UserName>      |                                                                                                  Especifica o nome do usuário para o qual você deseja receber a mensagem.                                                                                                   |
|    <SessionName>     |                                                                                                 Especifica o nome da sessão na qual você deseja receber a mensagem.                                                                                                 |
|     <SessionID>      |                                                                                            Especifica a ID numérica da sessão cujo usuário você deseja receber uma mensagem.                                                                                            |
|     @<FileName>      |                                                                         Identifica um arquivo que contém uma lista de nomes de usuário, nomes de sessão e IDs de sessão que você deseja receber a mensagem.                                                                         |
|          \*          |                                                                                                           Envia a mensagem a todos os nomes de usuário no sistema.                                                                                                            |
| /server:<ServerName> |                                              Especifica o servidor de host da sessão da área de trabalho remota cuja sessão ou usuário você deseja receber a mensagem. Se não for especificado, o **/Server** usará o servidor no qual você está conectado no momento.                                              |
|   /Time: <Seconds>    | Especifica a quantidade de tempo que a mensagem enviada é exibida na tela do usuário. Depois que o limite de tempo é atingido, a mensagem desaparece. Se nenhum limite de tempo for definido, a mensagem permanecerá na tela do usuário até que o usuário veja a mensagem e clique em **OK**. |
|          /v          |                                                                                                         Exibe informações sobre as ações que estão sendo executadas.                                                                                                         |
|          /w          |         Aguarda uma confirmação do usuário de que a mensagem foi recebida. Use esse parâmetro com **/Time:** <*segundos*> para evitar um longo atraso possível se o usuário não responder imediatamente. O uso desse parâmetro com **/v** também é útil.          |
|      <Message>       |                  Especifica o texto da mensagem que você deseja enviar. Se nenhuma mensagem for especificada, você será solicitado a inserir uma mensagem. Para enviar uma mensagem contida em um arquivo, digite o símbolo menor que (<) seguido pelo nome do arquivo.                  |
|          /?          |                                                                                                                  Exibe a ajuda no prompt de comando.                                                                                                                   |

## <a name="remarks"></a>Comentários
-   Se você não especificar um usuário ou uma sessão, **msg** exibirá uma mensagem de erro. Ao especificar uma sessão, ela deve ser uma ativa.
-   O usuário deve ter a permissão acesso especial à mensagem para enviar uma mensagem.

## <a name="BKMK_examples"></a>Disso
-   Para enviar a mensagem denominada "Vamos atender às às 13:00 hoje" para todas as sessões do Usuário1, digite:
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
-   Para enviar a mensagem a todas as sessões contidas na lista de userfile, digite:
    ```
    msg @userlist Let's meet at 1PM today
    ```
-   Para enviar a mensagem a todos os usuários que estão conectados, digite:
    ```
    msg * Let's meet at 1PM today
    ```
-   Para enviar a mensagem a todos os usuários, com um tempo limite de confirmação (por exemplo, 10 segundos), digite:
    ```
    msg * /time:10 Let's meet at 1PM today
    ```

#### <a name="additional-references"></a>Referências adicionais
-  [Chave da sintaxe de linha de comando](command-line-syntax-key.md)
-  [Serviços de Área de Trabalho Remota &#40;referência de&#41; comando de serviços de terminal](remote-desktop-services-terminal-services-command-reference.md)
