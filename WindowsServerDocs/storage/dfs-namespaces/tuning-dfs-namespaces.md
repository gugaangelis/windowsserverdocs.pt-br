---
title: Ajustando namespaces de DFS
description: Este artigo descreve como ajustar ou otimizar namespaces de DFS
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 011512deaeb99ded7d0bfc32a48f19ab3b622475
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386153"
---
# <a name="tuning-dfs-namespaces"></a>Ajustando namespaces de DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Depois de criar um namespace e adicionar pastas e destinos, consulte as seções a seguir para ajustar ou otimizar a maneira como o namespace do DFS manipula referências e pesquisas Active Directory Domain Services (AD DS) para dados de namespace atualizados:

-   [Habilitar a enumeração baseada em acesso em um namespace](enable-access-based-enumeration-on-a-namespace.md)
-   [Habilitar ou desabilitar referências e failback de cliente](enable-or-disable-referrals-and-client-failback.md)
-   [Alterar a quantidade de tempo que os clientes armazenam referências](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Definir o método de ordenação dos destinos nas referências](set-the-ordering-method-for-targets-in-referrals.md)
-   [Definir a prioridade de destino para sobrepor a ordem de referência](set-target-priority-to-override-referral-ordering.md)
-   [Otimizar a sondagem de namespace](optimize-namespace-polling.md)
-   [Usando permissões herdadas com enumeração baseada em acesso](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Para procurar pastas ou destinos de pasta, selecione um namespace, clique no **pesquisa** guia, digite sua cadeia de caracteres de pesquisa na caixa de texto e, em seguida, clique em **pesquisa**.