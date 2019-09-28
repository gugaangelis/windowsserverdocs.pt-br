---
title: sessão de consulta
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: bde9a246f2c46eaa466f2863c2cfc3c28a3a04eb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384912"
---
# <a name="query-session"></a>sessão de consulta

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre sessões em um servidor de Host da Sessão da Área de Trabalho Remota (host de sessão da área de trabalho remota).
A lista inclui informações não apenas sobre sessões ativas, mas também sobre outras sessões que o servidor executa.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
> ## <a name="syntax"></a>Sintaxe
> ```
> query session [<SessionName> | <UserName> | <SessionID>] [/server:<ServerName>] [/mode] [/flow] [/connect] [/counter]
> ```
> ## <a name="parameters"></a>Parâmetros
> 
> |      Parâmetro       |                                                      Descrição                                                      |
> |----------------------|-----------------------------------------------------------------------------------------------------------------------|
> |    <SessionName>     |                               Especifica o nome da sessão que você deseja consultar.                               |
> |      <UserName>      |                           Especifica o nome do usuário cujas sessões você deseja consultar.                            |
> |     <SessionID>      |                                Especifica a ID da sessão que você deseja consultar.                                |
> | /server:<ServerName> |                  Identifica o servidor host da sessão da área de trabalho remota a ser consultado. O padrão é o servidor atual.                   |
> |        /Mode         |                                            Exibe as configurações de linha atuais.                                            |
> |        /flow         |                                        Exibe as configurações de controle de fluxo atuais.                                        |
> |       /connect       |                                          Exibe as configurações de conexão atuais.                                           |
> |       /Counter       | Exibe informações de contadores atuais, incluindo o número total de sessões criadas, desconectadas e reconectadas. |
> |          /?          |                                         Exibe a ajuda no prompt de comando.                                          |
> 
> ## <a name="remarks"></a>Comentários
> - Um usuário sempre pode consultar a sessão à qual o usuário está conectado no momento. Para consultar outras sessões, o usuário deve ter informações de consulta permissão de acesso especial.
> - Se você não especificar uma sessão usando <*sessionname*>, < nome de*usuário*> ou <*SessionID*>, a **sessão de consulta** exibirá informações sobre todas as sessões ativas no sistema.
> - Quando a **sessão de consulta** retorna informações, um símbolo maior que (>) é exibido antes da sessão atual. Veja a seguir uma saída de exemplo para a **sessão de consulta**:
>   ```
>   C:\>query session
>    SESSIONNAME    USERNAME       ID STATE  TYPE   DEVICE
>   console        Administrator1  0 active wdcon
>    rdp-tcp#1      User1           1 active wdtshare
>    rdp-tcp                        2 listen wdtshare
>                                   4 idle
>                                   5 idle
>   ```
>   O símbolo maior que (>) indica a sessão atual. SESSIONname especifica o nome atribuído à sessão. USERNAME indica o nome de usuário do usuário conectado à sessão. O estado fornece informações sobre o estado atual da sessão. TIPO indica o tipo de sessão. O dispositivo, que não está presente no console ou nas sessões conectadas à rede, é o nome do dispositivo atribuído à sessão. O comentário que as informações de sessão a seguir é do perfil de sessão. Todas as sessões em que o estado inicial é configurado como DESABILITAdo não aparecem na lista **sessão de consulta** até que elas sejam habilitadas.
>   ## <a name="BKMK_examples"></a>Disso
> - Para exibir informações sobre todas as sessões ativas no servidor Servidor2, digite:
>   ```
>   query session /server:SERver2
>   ```
> - Para exibir informações sobre a sessão ativa modeM02, digite:
>   ```
>   query session modeM02
>   ```
>   #### <a name="additional-references"></a>Referências adicionais
>   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
>   [consulta](query.md)
>   [ &#40;serviços de área de trabalho remota&#41; referência de comando de serviços de terminal](remote-desktop-services-terminal-services-command-reference.md)
