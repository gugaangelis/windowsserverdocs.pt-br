---
title: Criar uma pasta em um namespace DFS
description: Este artigo descreve como criar uma pasta em um namespace DFS.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ca9fa0b87c6e995f3f0c38abec80fef9068df90
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="create-a-folder-in-a-dfs-namespace"></a>Criar uma pasta em um namespace DFS

> Aplicável a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Você pode usar pastas para criar níveis adicionais de hierarquia em um namespace. Você também pode criar pastas com destinos de pasta para adicionar pastas compartilhadas ao namespace. Pastas DFS com destinos de pasta não pode conter outras pastas DFS, portanto se você quiser adicionar um nível de hierarquia ao namespace, não adicione destinos de pasta para a pasta.

Use o procedimento a seguir para criar uma pasta em um namespace usando o Gerenciamento DFS:

## <a name="to-create-a-folder-in-a-dfs-namespace"></a>Para criar uma pasta em um namespace DFS

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, no nó **Namespaces**, clique com o botão direito do mouse em um namespace ou pasta dentro de um, clique em **Nova pasta**.

3.  Na caixa de texto **Nome**, digite o nome da nova pasta.

4.  Para adicionar um ou mais destinos de pasta para a pasta, clique em **adicionar** e especifique o caminho da convenção de nomenclatura Universal (UNC) do destino da pasta e clique em **OK**.


> [!TIP]
> Para criar uma pasta em um namespace usando Windows PowerShell, use o [New-DfsnFolder](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnfolder) cmdlet. O módulo do Windows PowerShell de DFSN foi apresentado no Windows Server 2012.


## <a name="see-also"></a>Veja também

-   [Implantando namespaces DFS](deploying-dfs-namespaces.md)
-   [Delegar permissões de gerenciamento para Namespaces DFS](delegate-management-permissions-for-dfs-namespaces.md)


