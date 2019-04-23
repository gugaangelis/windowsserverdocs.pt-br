---
title: Replicar destinos de pasta usando replicação DFS
description: Este artigo descreve como replicar destinos de pasta usando replicação DFS
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 098ffd1a47891d2355742682a002f80dcfc59650
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878077"
---
# <a name="replicate-folder-targets-using-dfs-replication"></a>Replicar destinos de pasta usando a replicação DFS

> Aplica-se a: 2019 do Windows Server, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 e Windows Server 2008

Você pode usar a replicação DFS para manter o conteúdo de destinos de pasta em sincronia para que os usuários vejam os mesmos arquivos independentemente de qual o computador cliente de destino de pasta é chamado.

## <a name="to-replicate-folder-targets-using-dfs-replication"></a>Para replicar alvos de pasta usando replicação de DFS

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique com o botão direito do mouse em uma ou mais pastas com destinos e, em seguida, clique em **Replicar pasta**.

3.  Siga as instruções no Assistente de replicação de pasta.

> [!NOTE]
> Alterações de configuração não são aplicadas imediatamente a todos os membros, exceto quando usando o [Suspend-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/suspend-dfsreplicationgroup) e [Sync-DfsReplicationGroup](https://technet.microsoft.com/itpro/powershell/windows/dfsr/sync-dfsreplicationgroup) cmdlets. A nova configuração deve ser replicada para todos os controladores de domínio e cada membro no grupo de replicação deve pesquisar seu controlador de domínio mais próximo para obter as alterações. A quantidade de tempo necessário para isso depende da latência de replicação do Active Directory Directory Services (AD DS) e do longo intervalo de sondagem (60 minutos) em cada membro. Para pesquisar imediatamente as alterações de configuração, abra uma janela de prompt de comando e, em seguida, digite o seguinte comando de uma vez para cada membro do grupo de replicação: <br /> dfsrdiag.exe PollAD /Member:DOMAIN\Server1
<br />
Para fazer isso uma sessão do Windows PowerShell, use o [DfsrConfigurationFromAD atualização](https://technet.microsoft.com/itpro/powershell/windows/dfsr/update-dfsrconfigurationfromad) cmdlet, que foi introduzido no Windows Server 2012 R2.

## <a name="see-also"></a>Consulte também

-   [Implantando os Namespaces do DFS](deploying-dfs-namespaces.md)
-   [Delegar permissões de gerenciamento para Namespaces do DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicação do DFS](../dfs-replication/dfsr-overview.md)