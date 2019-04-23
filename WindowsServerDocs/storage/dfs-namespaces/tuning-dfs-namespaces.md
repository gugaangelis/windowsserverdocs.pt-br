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
ms.openlocfilehash: c11bbf65c3baebebe1e5143a5e694ca752500aca
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844627"
---
# <a name="tuning-dfs-namespaces"></a>Ajustando namespaces de DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Depois de criar um namespace e adicionar pastas e destinos, consulte as seções a seguir para ajustar ou otimizar a maneira como o Namespace de DFS trata referências e controla os serviços de domínio Active Directory (AD DS) para dados do namespace atualizado:

-   [Habilitar enumeração baseada em acesso em um Namespace](enable-access-based-enumeration-on-a-namespace.md)
-   [Habilitar ou desabilitar referências e Failback de cliente](enable-or-disable-referrals-and-client-failback.md)
-   [Alterar a quantidade de tempo que os clientes armazenam as referências](change-the-amount-of-time-that-clients-cache-referrals.md)
-   [Definir o método de ordenação dos destinos nas referências](set-the-ordering-method-for-targets-in-referrals.md)
-   [Definir a prioridade de destino para substituir ordenação de referências](set-target-priority-to-override-referral-ordering.md)
-   [Otimizar a chamada seletiva do Namespace](optimize-namespace-polling.md)
-   [Usando as permissões herdadas com a enumeração baseada em acesso](using-inherited-permissions-with-access-based-enumeration.md)

> [!NOTE]
> Para procurar pastas ou destinos de pasta, selecione um namespace, clique no **pesquisa** guia, digite sua cadeia de caracteres de pesquisa na caixa de texto e, em seguida, clique em **pesquisa**.