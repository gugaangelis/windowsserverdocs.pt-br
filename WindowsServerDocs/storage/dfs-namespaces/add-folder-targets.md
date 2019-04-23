---
title: Adicionar pasta de destino
description: Este tópico descreve como adicionar destinos de pasta (caminhos UNC)
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms-date: 06/05/2017
ms.openlocfilehash: 8cc61189076669d5c24244294b2f0eee2b783517
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830997"
---
# <a name="add-folder-targets"></a>Adicionar destinos de pasta

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Um destino de pasta é o caminho UNC (Convenção de Nomenclatura Universal) de uma pasta compartilhada ou outro namespace associado a uma pasta em um namespace. A adição de vários destinos de pasta aumenta a disponibilidade da pasta no namespace.

## <a name="to-add-a-folder-target"></a>Para adicionar uma pasta alvo

Para adicionar uma pasta usando gerenciamento DFS, use o procedimento a seguir:

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique com o botão direito do mouse em uma pasta e, em seguida, clique em **Adicionar alvo de pasta**.

3.  Digite o caminho para o destino de pasta ou clique em **procurar** para localizar o destino de pasta.

4.  Se a pasta é replicada usando a replicação DFS, você pode especificar se você deseja adicionar o novo destino de pasta para o grupo de replicação.

> [!TIP]
> Para adicionar um alvo de pasta usando o Windows PowerShell, use o [New-DfsnFolderTarget](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfoldertarget). O módulo do Windows PowerShell de DFSN foi apresentado no Windows Server 2012.

> [!NOTE]
> As pastas podem conter destinos de pasta ou outras pastas DFS, mas não ambos, no mesmo nível na hierarquia de pastas.

## <a name="see-also"></a>Consulte também

-   [Implantando os Namespaces do DFS](deploying-dfs-namespaces.md)
-   [Delegar permissões de gerenciamento para Namespaces do DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicar destinos de pasta usando a replicação do DFS](replicate-folder-targets-using-dfs-replication.md)