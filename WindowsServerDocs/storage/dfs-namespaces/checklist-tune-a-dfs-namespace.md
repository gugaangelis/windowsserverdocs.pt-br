---
title: Lista de verificação - Ajustar um Namespace DFS
description: Este artigo descreve como otimizar a como o Namespace de DFS manipula indicações e sonda o AD DS para dados do namespace atualizado
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5e2d43f75a64a6a7950539c14386c6a037bc3c45
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872637"
---
# <a name="checklist-tune-a-dfs-namespace"></a>Lista de verificação: Ajustar um namespace do DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Depois de criar um namespace e adicionar pastas e destinos, use a seguinte lista de verificação para ajustar ou otimizar a forma como o namespace do DFS lida com as referências e controla os serviços de domínio do Active Directory (AD DS) para dados do namespace atualizado.

-   Impedir que os usuários vejam pastas em um namespace que não têm permissões para acessar. [Habilitar enumeração baseada em acesso em um Namespace](enable-access-based-enumeration-on-a-namespace.md) 
-   Permitir ou impedir que os usuários sejam referidos a um namespace ou uma pasta de destino quando acessam uma pasta no namespace. [Habilitar ou desabilitar referências e Failback de cliente](enable-or-disable-referrals-and-client-failback.md) 
-   Ajustar quanto tempo os clientes armazenam uma referências antes de solicitar uma nova. [Alterar a quantidade de tempo que os clientes armazenam as referências](change-the-amount-of-time-that-clients-cache-referrals.md)
-   Otimize como servidores de namespace sondagem o AD DS para obter os dados de namespace mais atuais. [Otimizar a chamada seletiva do Namespace](optimize-namespace-polling.md)
-   Use as permissões herdadas para controlar quais usuários podem exibir pastas em um namespace para o qual a enumeração baseada em acesso está habilitada. [Usando as permissões herdadas com a enumeração baseada em acesso](using-inherited-permissions-with-access-based-enumeration.md)

Além disso, ao usar um aperfeiçoamento de Namespaces do DFS conhecido como prioridade de destino, você pode especificar a prioridade de servidores para que um servidor específico é sempre colocado primeiro ou último lugar na lista de servidores (conhecido como referência) que o cliente recebe quando ele acessa uma pasta com destinos no namespace.

-   Especifica em que ordem os usuários devem ser referidos a destinos de pasta. [Definir o método de ordenação dos destinos nas referências](set-the-ordering-method-for-targets-in-referrals.md)
-   Substitua ordenação de referência para um destino de servidor ou uma pasta específica de namespace. [Definir a prioridade de destino para substituir ordenação de referências](set-target-priority-to-override-referral-ordering.md)

## <a name="see-also"></a>Consulte também

-   [Namespaces](https://technet.microsoft.com/library/cc771914(v=ws.11).aspx)
-   [Lista de verificação: Implante Namespaces DFS](checklist-deploy-dfs-namespaces.md)
-   [Ajuste os Namespaces do DFS](tuning-dfs-namespaces.md)


