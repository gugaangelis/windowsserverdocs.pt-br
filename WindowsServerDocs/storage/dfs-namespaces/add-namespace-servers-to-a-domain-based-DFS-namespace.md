---
title: Adicionar servidores de namespace para um namespace DFS baseado em domínio
description: Este artigo descreve como especificar os servidores de namespace adicionais para hospedar um namespace usando o Gerenciamento DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: fa28ec06448b67724ce8e3f29894fc70f6a6dae9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386251"
---
# <a name="add-namespace-servers-to-a-domain-based-dfs-namespace"></a>Adicionar servidores de namespace para um namespace DFS baseado em domínio

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Você pode aumentar a disponibilidade de um namespace baseado em domínio, especificando os servidores de namespace adicionais para hospedar o namespace.

## <a name="to-add-a-namespace-server-to-a-domain-based-namespace"></a>Para adicionar servidores de namespace para um namespace baseado em domínio

Para adicionar um servidor de namespace para um namespace baseado em domínio usando o Gerenciamento DFS, use o procedimento a seguir:

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique com o botão direito do mouse em um namespace baseado em domínio e, em seguida, clique em **Adicionar servidor de namespace**.

3.  Digite o caminho para outro servidor ou clique em **procurar** para localizar um servidor.

> [!NOTE]
> Este procedimento não é aplicável para namespaces autônomos porque aceitam apenas um servidor de namespace. Para aumentar a disponibilidade de um namespace autônomo, especifique um cluster de failover como o servidor de namespace no Assistente de novo Namespace.


> [!TIP]
> Para adicionar um servidor de namespace usando o Windows PowerShell, use o [cmdlet New-DfsnRootTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroottarget). O módulo do Windows PowerShell de DFSN foi apresentado no Windows Server 2012.

## <a name="see-also"></a>Consulte também

-   [Implantar namespaces do DFS](deploying-dfs-namespaces.md)
-   [Examinar os requisitos do servidor de namespaces do DFS](https://technet.microsoft.com/library/cc753448(v=ws.11).aspx)
-   [Criar um namespace do DFS](create-a-dfs-namespace.md)
-   [Delegar permissões de gerenciamento para namespaces do DFS](delegate-management-permissions-for-dfs-namespaces.md)

