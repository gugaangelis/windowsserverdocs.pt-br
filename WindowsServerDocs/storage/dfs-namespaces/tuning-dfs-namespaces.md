---
title: Ajustando namespaces de DFS
description: Este artigo descreve como ajustar ou otimizar namespaces de DFS
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 4614441fc54913ba5a8b547bbf1ad3e8ce7ee69b
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="tuning-dfs-namespaces"></a>Ajustando namespaces de DFS

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Depois de criar um namespace e adicionar pastas e destinos, use esta lista de verificação para ajustar ou otimizar a maneira como o Namespace de DFS manipula indicações e controla os serviços do Active Directory Domain Services (AD DS) para dados do namespace atualizado.

-   [Habilitar a enumeração baseada em acesso em um Namespace](enable-access-based-enumeration-on-a-namespace.md)
-   [Habilitar ou desabilitar referências e Failback de cliente](enable-or-disable-referrals-and-client-failback.md)
-   [Mudar o tempo que os clientes armazenam as referências](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Defina o método de ordenação dos destinos nas referências](set-the-ordering-method-for-targets-in-referrals.md)
-   [Definir prioridade alvo para sobrepor ordem de referência](set-target-priority-to-override-referral-ordering.md)
-   [Otimizar sondagem de namespace](optimize-namespace-polling.md)
-   [Usando permissões herdadas com enumeração baseada em acesso](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Para procurar pastas ou destinos de pasta, selecione um namespace, clique no **pesquisa** guia, digite sua cadeia de caracteres de pesquisa na caixa de texto e, em seguida, clique em **pesquisa**.