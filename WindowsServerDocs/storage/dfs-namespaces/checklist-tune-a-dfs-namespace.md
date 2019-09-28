---
title: Lista de verificação - Ajustar um Namespace DFS
description: Este artigo descreve como otimizar a como o Namespace de DFS manipula indicações e sonda o AD DS para dados do namespace atualizado
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 8a4c581801cbb1dcfe3db2813fb66fb4514d6434
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402189"
---
# <a name="checklist-tune-a-dfs-namespace"></a>Lista de verificação: Ajustar um namespace do DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Depois de criar um namespace e adicionar pastas e destinos, use a seguinte lista de verificação para ajustar ou otimizar a maneira como o namespace do DFS manipula referências e pesquisas Active Directory Domain Services (AD DS) para dados de namespace atualizados.

-   Impedir que os usuários vejam pastas em um namespace que não têm permissões para acessar. [Habilitar a enumeração baseada em acesso em um namespace](enable-access-based-enumeration-on-a-namespace.md) 
-   Permitir ou impedir que os usuários sejam referidos a um namespace ou uma pasta de destino quando acessam uma pasta no namespace. [Habilitar ou desabilitar referências e failback de cliente](enable-or-disable-referrals-and-client-failback.md) 
-   Ajustar quanto tempo os clientes armazenam uma referências antes de solicitar uma nova. [Alterar a quantidade de tempo que os clientes armazenam referências](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Otimize como servidores de namespace sondagem o AD DS para obter os dados de namespace mais atuais. [Otimizar a sondagem de namespace](optimize-namespace-polling.md)
-   Use as permissões herdadas para controlar quais usuários podem exibir pastas em um namespace para o qual a enumeração baseada em acesso está habilitada. [Usando permissões herdadas com enumeração baseada em acesso](using-inherited-permissions-with-access-based-enumeration.md)

Além disso, usando um aprimoramento de namespaces do DFS conhecido como prioridade de destino, você pode especificar a prioridade dos servidores para que um servidor específico seja sempre posicionado primeiro ou último na lista de servidores (conhecida como referência) que o cliente recebe quando acessa uma pasta com destinos no namespace.

-   Especifica em que ordem os usuários devem ser referidos a destinos de pasta. [Definir o método de ordenação dos destinos nas referências](set-the-ordering-method-for-targets-in-referrals.md)
-   Substitua ordenação de referência para um destino de servidor ou uma pasta específica de namespace. [Definir a prioridade de destino para sobrepor a ordem de referência](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>Consulte também

-   [Namespaces](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Lista de verificação: implantar Namespaces do DFS](checklist-deploy-dfs-namespaces.md)
-   [Ajustar namespaces do DFS](tuning-dfs-namespaces.md)


