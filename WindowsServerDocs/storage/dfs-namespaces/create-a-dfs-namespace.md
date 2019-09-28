---
title: Crie um namespace DFS
description: Este artigo descreve como criar um namespace DFS.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f4d4b86dd1a105576ac4d1749213696b319ba528
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402206"
---
# <a name="create-a-dfs-namespace"></a>Crie um namespace DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Para criar um novo namespace, você pode usar o Gerenciador do servidor para criar o namespace quando você instala o serviço de função Namespaces DFS. Você também pode usar o [cmdlet New-DfsnRoot](https://docs.microsoft.com/powershell/module/dfsn/new-dfsnroot) em uma sessão do Windows PowerShell. 

O módulo do Windows PowerShell de DFSN foi apresentado no Windows Server 2012. 

Alernatively, você pode usar o procedimento a seguir para criar um namespace depois de instalar o serviço de função.

## <a name="to-create-a-namespace"></a>Para criar um namespace

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, clique com o botão direito no nó **Namespaces** em um namespace e, em seguida, clique em **Novo namespace**.

3.  Siga as instruções no **Assistente de Novo Namespace**.

    Para criar um namespace autônomo em um cluster de failover, especifique o nome de uma instância de servidor de arquivos em cluster no **servidor de Namespace** página do **novo Assistente de Namespace**.

> [!IMPORTANT]
> Não tente criar um namespace baseado em domínio usando o modo do Windows Server 2008, a menos que o nível funcional da floresta seja o Windows Server 2003 ou superior. Isso pode resultar em um namespace para o qual não é possível excluir pastas do DFS, produzindo a seguinte mensagem de erro: "A pasta não pode ser excluída. Não foi possível completar essa função".

## <a name="see-also"></a>Consulte também

-   [Implantar namespaces do DFS](deploying-dfs-namespaces.md)
-   [Escolher um tipo de namespace](choose-a-namespace-type.md)
-   [Adicionar servidores de namespace a um namespace do DFS baseado em domínio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [Delegar permissões de gerenciamento para Namespaces DFS](delegate-management-permissions-for-dfs-namespaces.md)


