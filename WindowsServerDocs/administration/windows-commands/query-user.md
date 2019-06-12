---
title: usuário de consulta
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c6e0d936e03e14e134c7bfd450a878fda1a796c4
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442005"
---
# <a name="query-user"></a>usuário de consulta

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre as sessões de usuário em um servidor de Host de sessão de área de trabalho remota (Host de sessão rd).
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
> ## <a name="syntax"></a>Sintaxe
> ```
> query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
> ```
> ## <a name="parameters"></a>Parâmetros
> 
> |      Parâmetro       |                                                     Descrição                                                     |
> |----------------------|---------------------------------------------------------------------------------------------------------------------|
> |      <UserName>      |                            Especifica o nome de logon do usuário que você deseja consultar.                             |
> |    <SessionName>     |                              Especifica o nome da sessão que você deseja consultar.                              |
> |     <SessionID>      |                               Especifica a ID da sessão que você deseja consultar.                               |
> | /server:<ServerName> | Especifica o servidor de Host de sessão de área de trabalho remota que você deseja consultar. Caso contrário, o servidor de Host de sessão de área de trabalho remota atual é usado. |
> |          /?          |                                        Exibe a ajuda no prompt de comando.                                         |
> 
> ## <a name="remarks"></a>Comentários
> - Você pode usar esse comando para descobrir se um usuário específico é conectado a uma servidor Host de sessão da área de trabalho específica. **Consultar usuário** retorna as seguintes informações:
>   -   O nome do usuário
>   -   O nome da sessão no servidor de Host de sessão de área de trabalho remota
>   -   A ID de sessão
>   -   O estado da sessão (ativa ou desconectada)
>   -   O tempo ocioso (o número de minutos desde a última pressionamento de tecla ou movimento do mouse na sessão)
>   -   A data e hora que o usuário conectado
> - Para usar **consultar usuário**, você deve ter permissão de controle total ou permissão de acesso especial de informações de consulta.
> - Se você usar **usuário de consulta** sem especificar <*nome de usuário*>, <*SessionName*>, ou <*SessionID*>, uma lista de todos os usuários que fizerem logon no servidor é retornado. Como alternativa, você também pode usar **sessão de consulta** para exibir uma lista de todas as sessões em um servidor.
> - Quando **consultar usuário** retorna informações, um símbolo de maior que (>) são exibidas antes da sessão atual.
> - O **/server** parâmetro é necessário apenas se você usar **consultar usuário** de um servidor remoto.
>   ## <a name="BKMK_examples"></a>Exemplos
> - Para exibir informações sobre todos os usuários registrados no sistema, digite:
>   ```
>   query user
>   ```
> - Para exibir informações sobre o usuário Usuário1 no servidor1, digite:
>   ```
>   query user USER1 /server:SERver1
>   ```
>   #### <a name="additional-references"></a>Referências adicionais
>   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
>   [consulta](query.md)
>   [Remote Desktop Services &#40;serviços de Terminal&#41; referência de comandos](remote-desktop-services-terminal-services-command-reference.md)
