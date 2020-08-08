---
title: Replicar destinos de pasta usando replicação DFS
description: Este artigo descreve como replicar destinos de pasta usando replicação DFS
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8345d12c77af92999d64f63809752180a3ea91bc
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954743"
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>Replicar destinos de pasta usando a replicação DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

Você pode usar a replicação DFS para manter o conteúdo de destinos de pasta em sincronia para que os usuários vejam os mesmos arquivos independentemente de qual o computador cliente de destino de pasta é chamado.

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>Para replicar alvos de pasta usando replicação de DFS

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique com o botão direito do mouse em uma ou mais pastas com destinos e, em seguida, clique em **Replicar pasta**.

3.  Siga as instruções no Assistente de replicação de pasta.

> [!NOTE]
> Alterações de configuração não são aplicadas imediatamente a todos os membros, exceto quando usando o [Suspend-DfsReplicationGroup](/powershell/module/dfsr/suspend-dfsreplicationgroup?view=win10-ps) e [Sync-DfsReplicationGroup](/powershell/module/dfsr/sync-dfsreplicationgroup?view=win10-ps) cmdlets. A nova configuração deve ser replicada para todos os controladores de domínio e cada membro no grupo de replicação deve pesquisar seu controlador de domínio mais próximo para obter as alterações. O tempo necessário depende da latência de replicação do Active Directory Services (AD DS) e do intervalo de sondagem longo (60 minutos) em cada membro. Para pesquisar imediatamente as alterações de configuração, abra uma janela de prompt de comando e, em seguida, digite o seguinte comando de uma vez para cada membro do grupo de replicação: <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
Para fazer isso a partir de uma sessão do Windows PowerShell, use o [Update-DfsrConfigurationFromAD](/powershell/module/dfsr/update-dfsrconfigurationfromad?view=win10-ps) cmdlet, que foi introduzido no Windows Server 2012 R2.

## <a name="additional-references"></a>Referências adicionais

-   [Implantar namespaces do DFS](deploying-dfs-namespaces.md)
-   [Delegar permissões de gerenciamento para namespaces do DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicação do DFS](../dfs-replication/dfsr-overview.md)
