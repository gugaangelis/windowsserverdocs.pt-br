---
title: Crie um namespace DFS
description: Este artigo descreve como criar um namespace DFS.
ms.date: 6/5/2017
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6999dec3681a765ac64fdedd2e695c8a3f7dbcfa
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939421"
---
# <a name="create-a-dfs-namespace"></a>Crie um namespace DFS

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Para criar um novo namespace, você pode usar o Gerenciador do servidor para criar o namespace quando você instala o serviço de função Namespaces DFS. Você também pode usar o [cmdlet New-DfsnRoot](/powershell/module/dfsn/new-dfsnroot) em uma sessão do Windows PowerShell.

O módulo do Windows PowerShell de DFSN foi apresentado no Windows Server 2012.

Alernatively, você pode usar o procedimento a seguir para criar um namespace depois de instalar o serviço de função.

## <a name="to-create-a-namespace"></a>Para criar um namespace

1.  Clique em **Iniciar**, aponte para **Ferramentas Administrativas** e clique em **Gerenciamento DFS**.

2.  Na árvore de console, clique com o botão direito no nó **Namespaces** em um namespace e, em seguida, clique em **Novo namespace**.

3.  Siga as instruções no **Assistente de Novo Namespace**.

    Para criar um namespace autônomo em um cluster de failover, especifique o nome de uma instância de servidor de arquivos em cluster no **servidor de Namespace** página do **novo Assistente de Namespace**.

> [!IMPORTANT]
> Não tente criar um namespace baseado em domínio usando o modo do Windows Server 2008, a menos que o nível funcional da floresta seja Windows Server 2003 ou posterior. Isso pode resultar em um namespace para os quais você não pode excluir pastas DFS, gerando a seguinte mensagem de erro: "a pasta não pode ser excluída. Não foi possível completar essa função".

## <a name="additional-references"></a>Referências adicionais

-   [Implantar namespaces do DFS](deploying-dfs-namespaces.md)
-   [Escolher um tipo de namespace](choose-a-namespace-type.md)
-   [Adicionar servidores de namespace a um namespace do DFS baseado em domínio](add-namespace-servers-to-a-domain-based-dfs-namespace.md)
-   [Delegar permissões de gerenciamento para namespaces do DFS](delegate-management-permissions-for-dfs-namespaces.md).
