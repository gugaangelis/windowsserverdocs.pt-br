---
title: change logon
description: 'Tópico de comandos do Windows para * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: c04eaffe366dce079aed53351589c1b5026954e3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71379637"
---
>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

# <a name="change-logon"></a>change logon
Habilita ou desabilita os logons de sessões de cliente ou exibe o status de logon atual.
Esse utilitário é útil para manutenção do sistema.
Para obter exemplos de como usar esse comando, consulte [exemplos](#BKMK_examples).
> [!NOTE]
> No Windows Server 2008 R2, os Serviços de Terminal foram renomeados como Serviços de Área de Trabalho Remota. Para descobrir as novidades da versão mais recente, consulte [novidades do serviços de área de trabalho remota no Windows server 2012](https://technet.microsoft.com/library/hh831527) na biblioteca do TechNet do Windows Server.
> ## <a name="syntax"></a>Sintaxe
> ```
> change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
> ```
> ## <a name="parameters"></a>Parâmetros
> 
> |     Parâmetro      |                                                       Descrição                                                        |
> |--------------------|--------------------------------------------------------------------------------------------------------------------------|
> |       /Query       |                             Exibe o status de logon atual, se habilitado ou desabilitado.                              |
> |      /Enable       |                              Habilita logons de sessões de cliente, mas não do console do.                              |
> |      /Disable      |  Desabilita logons subsequentes de sessões de cliente, mas não do console do. Não afeta usuários conectados no momento.   |
> |       /drain       |                 Desabilita logons de novas sessões de cliente, mas permite reconexões com sessões existentes.                 |
> | /drainuntilrestart | Desabilita logons de novas sessões de cliente até que o computador seja reiniciado, mas permite reconexões com sessões existentes. |
> |         /?         |                                           Exibe a ajuda no prompt de comando.                                           |
> 
> ## <a name="remarks"></a>Comentários
> - Somente os administradores podem usar o comando **Change login** .
> - Os logons são habilitados novamente quando o sistema é reiniciado. Se você estiver conectado ao servidor de Host da Sessão da Área de Trabalho Remota (host de sessão da área de trabalho remota) de uma sessão de cliente e desabilitar os logons e, em seguida, fazer logoff antes de reabilitar os logons, você não poderá se reconectar à sua sessão. Para reabilitar logons de sessões de cliente, faça logon no console do.
>   ## <a name="BKMK_examples"></a>Disso
> - Para exibir o status de logon atual, digite:
>   ```
>   change logon /query
>   ```
> - Para habilitar logons de sessões de cliente, digite:
>   ```
>   change logon /enable
>   ```
> - Para desabilitar os logons do cliente, digite:
>   ```
>   change logon /disable
>   ```
>   #### <a name="additional-references"></a>Referências adicionais
>   [Chave de sintaxe de linha de comando](command-line-syntax-key.md)
>   [alterar](change.md)
>   [ &#40;serviços de área de trabalho remota&#41; referência de comando de serviços de terminal](remote-desktop-services-terminal-services-command-reference.md)
