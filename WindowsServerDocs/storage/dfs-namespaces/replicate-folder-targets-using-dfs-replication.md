---
title: Replicar destinos de pasta usando replicação DFS
description: Este artigo descreve como replicar destinos de pasta usando replicação DFS
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: ad26b8685539869e9302eafac69df19d11778a1b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386127"
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>Replicar destinos de pasta usando a replicação DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

Você pode usar a replicação DFS para manter o conteúdo de destinos de pasta em sincronia para que os usuários vejam os mesmos arquivos independentemente de qual o computador cliente de destino de pasta é chamado.

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>Para replicar alvos de pasta usando replicação de DFS

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique com o botão direito do mouse em uma ou mais pastas com destinos e, em seguida, clique em **Replicar pasta**.

3.  Siga as instruções no Assistente de replicação de pasta.

> [!NOTE]
> Alterações de configuração não são aplicadas imediatamente a todos os membros, exceto quando usando o [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) e [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup) cmdlets. A nova configuração deve ser replicada para todos os controladores de domínio e cada membro no grupo de replicação deve pesquisar seu controlador de domínio mais próximo para obter as alterações. A quantidade de tempo que isso leva depende da latência de replicação do AD DS (Active Directory Directory Services) e do intervalo de sondagem longa (60 minutos) em cada membro. Para pesquisar imediatamente as alterações de configuração, abra uma janela de prompt de comando e, em seguida, digite o seguinte comando de uma vez para cada membro do grupo de replicação: <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
Para fazer isso em uma sessão do Windows PowerShell, use o cmdlet [Update-DfsrConfigurationFromAD](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad) , que foi introduzido no Windows Server 2012 R2.

## <a name="see-also"></a>Consulte também

-   [Implantar namespaces do DFS](deploying-dfs-namespaces.md)
-   [Delegar permissões de gerenciamento para namespaces do DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicação do DFS](../dfs-replication/dfsr-overview.md)