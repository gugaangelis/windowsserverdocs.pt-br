---
title: consultar usuário
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 65bf42281e6e1956331c061167aea23d1cd61a1d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384872"
---
# <a name="query-user"></a>consultar usuário

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre sessões de usuário em um servidor de Host da Sessão da Área de Trabalho Remota (host de sessão da área de trabalho remota).
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
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
> | /server:<ServerName> | Especifica o servidor de host da sessão da área de trabalho remota que você deseja consultar. Caso contrário, o servidor host da Sessão RD atual será usado. |
> |          /?          |                                        Exibe a ajuda no prompt de comando.                                         |
> 
> ## <a name="remarks"></a>Comentários
> - Você pode usar esse comando para descobrir se um usuário específico está conectado a um servidor host de sessão da área de trabalho remota. o **usuário de consulta** retorna as seguintes informações:
>   -   O nome do usuário
>   -   O nome da sessão no servidor de host da sessão da área de trabalho remota
>   -   A ID da sessão
>   -   O estado da sessão (ativa ou desconectada)
>   -   O tempo ocioso (o número de minutos desde o último pressionamento da tecla ou o movimento do mouse na sessão)
>   -   A data e a hora em que o usuário fez logon
> - Para usar o **usuário de consulta**, você deve ter permissão de controle total ou permissão de acesso especial para informações de consulta.
> - Se você usar o **usuário de consulta** sem especificar <*UserName*>, <*SessionName*> ou <*SessionID*>, será retornada uma lista de todos os usuários que fizeram logon no servidor. Como alternativa, você também pode usar a **sessão de consulta** para exibir uma lista de todas as sessões em um servidor.
> - Quando o **usuário da consulta** retorna informações, um símbolo maior que (>) é exibido antes da sessão atual.
> - O parâmetro **/Server** será necessário somente se você usar o **usuário de consulta** de um servidor remoto.
>   ## <a name="BKMK_examples"></a>Disso
> - Para exibir informações sobre todos os usuários conectados ao sistema, digite:
>   ```
>   query user
>   ```
> - Para exibir informações sobre o usuário usuário1 no servidor SERver1, digite:
>   ```
>   query user USER1 /server:SERver1
>   ```
>   #### <a name="additional-references"></a>Referências adicionais
>   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
>   [consulta](query.md)
>   [ &#40;serviços de área de trabalho remota&#41; referência de comando de serviços de terminal](remote-desktop-services-terminal-services-command-reference.md)
