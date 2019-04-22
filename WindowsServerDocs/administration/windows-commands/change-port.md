---
title: change port
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3d772c90-e849-4e74-b9ec-b6cae1159336 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 477761c35f08d5ec81adae80ba8f2fe9667786e9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818637"
---
# <a name="change-port"></a>change port

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

lista ou altera os mapeamentos de porta COM para ser compatível com aplicativos do MS-DOS.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
## <a name="syntax"></a>Sintaxe
```
change port [<PortX>=<PortY> | /d <PortX> | /query]
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|<PortX>=<PortY>|Mapeia COM <*PortX*> para <*PortY*>.|
|/d <PortX>|Exclui o mapeamento para COM <*PortX*>.|
|/Query|Exibe os mapeamentos de porta atual.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
-   A maioria dos aplicativos em MS-DOS dão suporte a apenas COM1 a portas seriais de COM4. O **alterar a porta** comando mapeia uma porta serial para um número de porta diferente, permitindo que os aplicativos que não dão suporte a números elevados COM portas acessar a porta serial. o remapeamento funciona apenas para a sessão atual e não é mantido se você fazer logoff de uma sessão e, em seguida, faça logon novamente.
-   Use **alterar a porta** sem parâmetros para exibir as portas COM disponíveis e seus mapeamentos atuais.
## <a name="BKMK_examples"></a>Exemplos
-   Para mapear COM12 para COM1 para uso por um aplicativo baseado no MS-DOS, digite:
    ```
    change port com12=com1
    ```
-   Para exibir os mapeamentos de porta atual, digite:
    ```
    change port /query
    ```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[alterar](change.md)
[Remote Desktop Services &#40;serviços de Terminal&#41; referência de comandos](remote-desktop-services-terminal-services-command-reference.md)
