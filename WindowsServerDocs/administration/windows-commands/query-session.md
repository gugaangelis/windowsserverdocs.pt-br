---
title: sessão de consulta
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abc0ace8-0b74-4b6e-a937-a78bb4b61a1f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1b96f7e6a6252b40e5a32910d161b1fa0ff94e11
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868887"
---
# <a name="query-session"></a>sessão de consulta

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre as sessões em um servidor de Host de sessão de área de trabalho remota (Host de sessão rd).
A lista inclui informações não apenas sobre sessões ativas, mas também sobre outras sessões executadas pelo servidor.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
## <a name="syntax"></a>Sintaxe
```
query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|<SessionName>|Especifica o nome da sessão que você deseja consultar.|
|<UserName>|Especifica o nome do usuário cujas sessões você deseja consultar.|
|<SessionID>|Especifica a ID da sessão que você deseja consultar.|
|/server:<ServerName>|Identifica o servidor de Host de sessão de área de trabalho remota à consulta. O padrão é o servidor atual.|
|/mode|Exibe as configurações de linha atual.|
|/Flow|Exibe as configurações de controle de fluxo atual.|
|/connect|Configurações de conexão exibe atual.|
|/counter|Exibe informações atuais sobre contadores, incluindo o número total de sessões criadas, desconectadas e reconectadas.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
-   Um usuário sempre pode consultar a sessão à qual o usuário está conectado no momento. Para consultar outras sessões, o usuário deve ter permissão de acesso especiais de informações de consulta.
-   Se você não especificar uma sessão usando <*SessionName*>, <*nome de usuário*>, ou <*SessionID*>, **consultar sessão** Exibe informações sobre todas as sessões ativas no sistema.
-   Quando **sessão de consulta** retorna informações, um símbolo de maior que (>) são exibidas antes da sessão atual. A seguir está um exemplo de saída de **sessão de consulta**:
    ```
    C:\>query session
     SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
    >console        Administrator1  0 active wdcon
     rdp-tcp#1      User1           1 active wdtshare
     rdp-tcp                        2 listen wdtshare
                                    4 idle
                                    5 idle
    ```
    O símbolo de maior que (>) indica que a sessão atual. SESSIONNAME Especifica o nome atribuído à sessão. Nome de usuário indica o nome de usuário do usuário conectado à sessão. ESTADO fornece informações sobre o estado atual da sessão. TIPO indica o tipo de sessão. DISPOSITIVO, que não está presente para as sessões de console ou conectados à rede, é o nome do dispositivo atribuído à sessão. É o comentário que segue as informações da sessão do perfil da sessão. Todas as sessões em que o estado inicial seja configurado como desativado não aparecem na **sessão de consulta** lista até que elas são habilitadas.
## <a name="BKMK_examples"></a>Exemplos
-   Para exibir informações sobre todas as sessões ativas no servidor, SERver2, digite:
    ```
    query session /server:SERver2
    ```
-   Para exibir informações sobre a sessão ativa modeM02, digite:
    ```
    query session modeM02
    ```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[consulta](query.md)
[Remote Desktop Services &#40;serviços de Terminal&#41; referência de comandos](remote-desktop-services-terminal-services-command-reference.md)
