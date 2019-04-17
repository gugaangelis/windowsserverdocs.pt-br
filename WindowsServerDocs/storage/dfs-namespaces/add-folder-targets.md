---
title: Adicionar pasta de destino
description: "Este tópico descreve como adicionar destinos de pasta (caminhos UNC)"
ms.prod: windows-server-threshold
ms.author: jgerend
ms.manager: brianlic
ms.technology: storage
ms.topic: article
author: jasongerend
ms-date: 06/05/2017
ms.openlocfilehash: 71305089553d622f54cb4e5608034edfaf7abc5f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="add-folder-targets"></a>Adicionar destinos de pasta

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

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

## <a name="see-also"></a>Veja também

-   [Implantando namespaces DFS](deploying-dfs-namespaces.md)
-   [Delegar permissões de gerenciamento para Namespaces DFS](delegate-management-permissions-for-dfs-namespaces.md)
-   [Replicar destinos de pasta usando replicação DFS](replicate-folder-targets-using-dfs-replication.md)