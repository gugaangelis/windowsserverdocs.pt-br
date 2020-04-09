---
title: consultar usuário
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a670fb78-c055-464a-b61d-3a85632c52c5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6624c559bc85263da955f993ae7e4ad7e8b9ee2d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836799"
---
# <a name="query-user"></a>consultar usuário

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe informações sobre sessões de usuário em um servidor de Host da Sessão da Área de Trabalho Remota (host de sessão da área de trabalho remota).
para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
> ## <a name="syntax"></a>Sintaxe
> ```
> query user [<UserName> | <SessionName> | <SessionID>] [/server:<ServerName>]
> ```
> ### <a name="parameters"></a>Parâmetros
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
>   ## <a name="examples"></a><a name=BKMK_examples></a>Disso
> - Para exibir informações sobre todos os usuários conectados ao sistema, digite:
>   ```
>   query user
>   ```
> - Para exibir informações sobre o usuário usuário1 no servidor SERver1, digite:
>   ```
>   query user USER1 /server:SERver1
>   ```
>   ## <a name="additional-references"></a>Referências adicionais
>   - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
>   
>   [consultar](query.md) a [referência de comando serviços de área de trabalho remota (serviços de terminal)](remote-desktop-services-terminal-services-command-reference.md)
