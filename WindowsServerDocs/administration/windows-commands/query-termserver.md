---
title: consulta termserver
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 14270092949baf37588059d592e6f92e694a1739
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66442093"
---
# <a name="query-termserver"></a>consulta termserver

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de todos os servidores de Host de sessão de área de trabalho remota (Host de sessão rd) na rede.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
> ## <a name="syntax"></a>Sintaxe
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ## <a name="parameters"></a>Parâmetros
> 
> |    Parâmetro     |                                                                        Descrição                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               Especifica o nome que identifica o servidor de Host de sessão de área de trabalho remota.                                               |
> | /domain:<Domain> | Especifica o domínio para consultar os servidores de terminal. Você não precisa especificar um domínio, se você estiver consultando o domínio no qual você está trabalhando no momento. |
> |     /address     |                                                  Exibe os endereços de rede e de nó para cada servidor.                                                  |
> |    /continue     |                                              Impede a pausa após cada tela de informações é exibida.                                               |
> |        /?        |                                                            Exibe a ajuda no prompt de comando.                                                            |
> 
> ## <a name="remarks"></a>Comentários
> - **Consultar termserver** pesquisará a rede para todas as conectados a servidores de Host de sessão de área de trabalho remota e retorna as seguintes informações:
>   - O nome do servidor
>   - A rede (e o endereço do nó se a opção /address for usada)
>     ## <a name="BKMK_examples"></a>Exemplos
> - Para exibir informações sobre todos os servidores de Host de sessão de área de trabalho remota na rede, digite:
>   ```
>   query termserver
>   ```
> - Para exibir informações sobre o servidor de Host de sessão de área de trabalho remota denominado servidor3, digite:
>   ```
>   query termserver Server3
>   ```
> - Para exibir informações sobre todos os servidores de Host de sessão de área de trabalho remota no domínio CONTOSO, digite:
>   ```
>   query termserver /domain:CONTOSO
>   ```
> - Para exibir o endereço de rede e de nó para o servidor de Host de sessão de área de trabalho remota chamado servidor3, digite:
>   ```
>   query termserver Server3 /address
>   ```
>   #### <a name="additional-references"></a>Referências adicionais
>   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
>   [consulta](query.md)
>   [Remote Desktop Services &#40;serviços de Terminal&#41; referência de comandos](remote-desktop-services-terminal-services-command-reference.md)
