---
title: termserver de consulta
description: Tópico de comandos do Windows para * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b89d3b4-236f-4376-90b6-939a0ec4b288
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d63bd158dad74203aa7ee3fd4e43dffb97c4c873
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836919"
---
# <a name="query-termserver"></a>termserver de consulta

>Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Exibe uma lista de todos os servidores de Host da Sessão da Área de Trabalho Remota (host de sessão da área de trabalho remota) na rede.
para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
> ## <a name="syntax"></a>Sintaxe
> ```
> query termserver [<ServerName>] [/domain:<Domain>] [/address] [/continue]
> ```
> ### <a name="parameters"></a>Parâmetros
> 
> |    Parâmetro     |                                                                        Descrição                                                                         |
> |------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
> |   <ServerName>   |                                               Especifica o nome que identifica o servidor de host da sessão da área de trabalho remota.                                               |
> | /domain:<Domain> | Especifica o domínio para consultar os servidores de terminal. Você não precisará especificar um domínio se estiver consultando o domínio no qual você está trabalhando no momento. |
> |     /address     |                                                  Exibe os endereços de rede e de nó para cada servidor.                                                  |
> |    /continue     |                                              Impede a pausa após a exibição de cada tela de informações.                                               |
> |        /?        |                                                            Exibe a ajuda no prompt de comando.                                                            |
> 
> ## <a name="remarks"></a>Comentários
> - o **Query termserver** pesquisa a rede em busca de todos os servidores de host da sessão da área de trabalho remota conectados e retorna as seguintes informações:
>   - O nome do servidor
>   - A rede (e o endereço do nó se a opção/address for usada)
>     ## <a name="examples"></a><a name=BKMK_examples></a>Disso
> - Para exibir informações sobre todos os servidores de host de sessão da área de trabalho remota na rede, digite:
>   ```
>   query termserver
>   ```
> - Para exibir informações sobre o servidor de host da sessão da área de trabalho remota chamado Server3, digite:
>   ```
>   query termserver Server3
>   ```
> - Para exibir informações sobre todos os servidores de host da sessão da área de trabalho remota no domínio CONTOSO, digite:
>   ```
>   query termserver /domain:CONTOSO
>   ```
> - Para exibir a rede e o endereço do nó para o servidor de host da sessão da área de trabalho remota chamado Server3, digite:
>   ```
>   query termserver Server3 /address
>   ```
>   ## <a name="additional-references"></a>Referências adicionais
>   - [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
>   
>   [consultar](query.md) a [referência de comando serviços de área de trabalho remota (serviços de terminal)](remote-desktop-services-terminal-services-command-reference.md)
