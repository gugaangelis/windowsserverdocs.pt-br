---
title: change logon
description: 'Tópico de comandos do Windows para * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8a5668618417ec315a452d911dbe15c02c30eeb1
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861137"
---
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="change-logon"></a>change logon
Habilita ou desabilita os logons de sessões de cliente ou exibe o status atual do logon.
Esse utilitário é útil para a manutenção do sistema.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir o que há de novo na versão mais recente, consulte [novidades novo nos serviços de área de trabalho remota no Windows Server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
## <a name="syntax"></a>Sintaxe
```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```
## <a name="parameters"></a>Parâmetros
|Parâmetro|Descrição|
|-------|--------|
|/Query|Exibe o status atual do logon, se habilitado ou desabilitado.|
|/enable|Permite que os logons de sessões de cliente, mas não do console.|
|/disable|Desabilita os logons subsequentes de sessões de cliente, mas não do console. Não afeta os usuários atualmente conectados.|
|/drain|Desabilita os logons de novas sessões de cliente, mas permite reconexões com sessões existentes.|
|/drainuntilrestart|Desabilita os logons de novas sessões de cliente até que o computador for reiniciado, mas permite reconexões com sessões existentes.|
|/?|Exibe a ajuda no prompt de comando.|
## <a name="remarks"></a>Comentários
-   Somente administradores podem usar o **alterar o logon** comando.
-   Os logons são reativados quando você reiniciar o sistema. Se você estiver conectado ao servidor Host da sessão da área de trabalho remota (Host de sessão rd) de uma sessão de cliente e desabilitar logons e, em seguida, fazer logoff antes de habilitar novamente os logons, você não poderá se reconectar à sua sessão. Para habilitar novamente os logons de sessões de cliente, faça logon no console.
## <a name="BKMK_examples"></a>Exemplos
-   Para exibir o status atual do logon, digite:
    ```
    change logon /query
    ```
-   Para habilitar logons de sessões de cliente, digite:
    ```
    change logon /enable
    ```
-   Para desabilitar logons de clientes, digite:
    ```
    change logon /disable
    ```
#### <a name="additional-references"></a>Referências adicionais
[Chave de sintaxe de linha de comando](command-line-syntax-key.md)
[alterar](change.md)
[Remote Desktop Services &#40;serviços de Terminal&#41; referência de comandos](remote-desktop-services-terminal-services-command-reference.md)
