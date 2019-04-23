---
title: tsprof
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 27047868-b706-4208-b7e0-1437a2325dd3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e17d60126125fcd4b10373133dd61ca0db030290
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836067"
---
# <a name="tsprof"></a>tsprof

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Copia as informações de configuração de usuário dos serviços de área de trabalho remota de um usuário para outro.
As informações de configuração de usuário dos serviços de área de trabalho remota são exibidas em extensões de serviços de área de trabalho remota para usuários e grupos e locais do Active Directory que os usuários e computadores.

**TSPROF** também pode definir o caminho de perfil para um usuário.

Para obter exemplos de como usar esse comando, consulte [Exemplos](#BKMK_examples).

> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.

## <a name="syntax"></a>Sintaxe
```
tsprof /update {/domain:<DomainName> | /local} /profile:<path> <UserName>
tsprof /copy {/domain:<DomainName> | /local} [/profile:<path>] <Src_usr> <Dest_usr>
tsprof /q {/domain:<DomainName> | /local} <UserName>
```

## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/update|Atualizações de informações de caminho para perfil <*nome de usuário*> no domínio <*DomainName*> para <*Caminho_de_perfil*>.|
|/Domain:\<DomainName >|Especifica o nome do domínio no qual a operação é aplicada.|
|/local|Aplica-se a operação somente a contas de usuário local.|
|/profile:\<path>|Especifica o caminho de perfil, conforme exibido em extensões de serviços de área de trabalho remota em usuários e grupos e locais do Active Directory que os usuários e computadores.|
|\<UserName>|Especifica o nome do usuário para quem você deseja atualizar ou consultar o caminho de perfil do servidor.|
|/copy|Copia informações de configuração de usuário \< *SourceUser*> para \< *DestinationUser*> e atualiza as informações de caminho de perfil para \<  *DestinationUser*> para \< *Caminho_de_perfil*>. Ambos \< *SourceUser*> e \< *DestinationUser*> deve ser local ou deve estar no domínio \< *DomainName*> .|
|\<Src_usr>|Especifica o nome do usuário de quem você deseja copiar as informações de configuração do usuário.|
|\<Dest_usr>|Especifica o nome do usuário a quem você deseja copiar as informações de configuração do usuário.|
|/q|Exibe o caminho atual do perfil do usuário para quem você deseja consultar o caminho de perfil do servidor.|
|/?|Exibe a ajuda no prompt de comando.|

## <a name="remarks"></a>Comentários
-   O **tsprof** comando só está disponível quando você instalou o serviço de função do Terminal Server em um computador executando o serviço de função do Windows Server 2008 ou o Host de sessão de área de trabalho remota em um computador executando o Windows Server 2008 R2.

## <a name="BKMK_examples"></a>Exemplos
-   Para copiar informações de configuração do usuário de LocalUser1 para LocalUser2, digite:
    ```
    tsprof /copy /local LocalUser1 LocalUser2
    ```
-   Para definir o caminho do perfil dos serviços de área de trabalho remota para LocalUser1 em um diretório chamado "c:\profiles", digite:
    ```
    tsprof /update /local /profile:c:\profiles LocalUser1
    ```

#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[serviços de área de trabalho remota &#40;serviços de Terminal&#41; referência do comando](remote-desktop-services-terminal-services-command-reference.md)
