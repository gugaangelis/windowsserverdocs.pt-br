---
title: Definir prioridade alvo para sobrepor ordem de referência
description: Este artigo descreve como definir a prioridade de destino para substituir a ordenação de referência
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f0a6496802d2be16e84ef62c41fea6f0ae9f6438
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386125"
---
# <a name="set-target-priority-to-override-referral-ordering"></a>Definir a prioridade de destino para sobrepor a ordem de referência

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Uma referência é uma lista ordenada de destinos que um computador cliente recebe de um controlador de domínio ou servidor de namespace quando o usuário acessa uma raiz de namespace ou pasta com destinos no namespace. Cada destino em uma referência é classificado de acordo com o método de ordenação da pasta ou raiz do namespace. 

Para refinar a ordenação dos destinos, você pode definir a prioridade em destinos individuais. Por exemplo, você pode especificar que o destino seja o primeiro ou o último entre todos, ou o primeiro (ou último) entre todos os destinos de custo igual.

## <a name="to-set-target-priority-on-a-root-target-for-a-domain-based-namespace"></a>Para definir a prioridade em um destino de raiz de um namespace baseado em domínio

Para definir a prioridade em um destino de raiz de um namespace baseado em domínio, use o seguinte procedimento.

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique no namespace baseado em domínio que possui os destinos de raiz nos quais você deseja definir prioridade.

3.  No painel **Detalhes**, na guia **Servidores de Namespace**, clique com o botão direito do mouse no destino de raiz com a prioridade que você deseja alterar e clique em **Propriedades**.

4.  Na guia **Avançado**, clique em **Substituir ordenação de referências** e, em seguida, clique na prioridade desejada.

    -   **Primeiro entre todos os destinos**  Especifica que os usuários devem sempre ser referidos a esse destino se o destino estiver disponível.
    -   **Último entre todos os destinos** Especifica que os usuários nunca devem ser referidos a esse destino, a menos que todos os outros destinos estejam indisponíveis.
    -   **Primeiro entre todos os destinos de custo igual**  Especifica que os usuários devem ser referidos a este destino antes de outros destinos de mesmo custo (o que normalmente significa outros destinos no mesmo lugar).
    -   **Último entre os destinos de custo igual**  Especifica que os usuários nunca devem ser referidos a este destino se houver outros destinos de mesmo custo (o que normalmente significa outros destinos no mesmo lugar).

## <a name="to-set-target-priority-on-a-folder-target"></a>Para definir a prioridade no destino de uma pasta

Para definir a prioridade no destino de uma pasta, use o seguinte procedimento:

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique na pasta dos destinos nos quais você deseja definir a prioridade.

3.  No painel **Detalhes**, na guia **Destinos de Pasta**, clique com o botão direito do mouse no destino de pasta com a prioridade que você deseja alterar e clique em **Propriedades**.

4.  Na guia **Avançado**, clique em **Substituir ordenação de referências** e, em seguida, clique na prioridade desejada.

> [!NOTE]
> Para definir prioridades de alvo usando Windows PowerShell, use o [Set-DfsnRootTarget](https://technet.microsoft.com/library/jj884266.aspx) and [Set-DfsnFolderTarget](https://technet.microsoft.com/library/jj884264.aspx) cmdlets com os parâmetros **ReferralPriorityClass** e **ReferralPriorityRank**. Esses cmdlets foram introduzidos no Windows Server 2012.

## <a name="see-also"></a>Consulte também

-   [Ajustar namespaces do DFS](tuning-dfs-namespaces.md)
-   [Delegar permissões de gerenciamento para namespaces do DFS](delegate-management-permissions-for-dfs-namespaces.md)