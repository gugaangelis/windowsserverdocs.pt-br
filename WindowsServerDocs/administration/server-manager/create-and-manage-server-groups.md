---
title: criar e gerenciar grupos de servidores
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9d5b1be8-49fd-4ff7-9580-e4ff21fe4b17
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32e20040e2cb075e447c0d03d48676c7011a5a92
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889477"
---
# <a name="create-and-manage-server-groups"></a>criar e gerenciar grupos de servidores

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como criar grupos personalizados definidos pelo usuário dos servidores no Gerenciador do servidor no Windows Server.

## <a name="BKMK_groups"></a>Grupos de servidores
Servidores que você adicionar ao pool de servidores são exibidos na **todos os servidores** página no Gerenciador do servidor. Você pode criar grupos personalizados de servidores adicionados. Grupos de servidores permitem exibir e gerenciar um subconjunto menor de seu pool de servidores como uma unidade lógica; Por exemplo, você pode criar um grupo chamado **servidores contábeis** para todos os servidores em sua organização do departamento de contabilidade, ou um grupo chamado **Chicago** para todos os servidores que estão localizados geograficamente em Chicago. Depois de criar um grupo de servidores, home page do grupo no Gerenciador de servidores exibe informações sobre eventos, serviços, contadores de desempenho, resultados do analisador de práticas recomendadas e instalou as funções e recursos para o grupo como um todo.

Os servidores podem ser membros de mais de um grupo.

#### <a name="to-create-a-new-server-group"></a>Para criar um novo grupo de servidores

1.  Sobre o **Manage** menu, clique em **criar grupo de servidores**.

2.  Na caixa de texto **Nome do grupo de servidores** , digite um nome fácil para seu grupo de servidores, como **Servidores Contábeis**.

3.  Adicionar servidores para o **selecionados** listar do pool de servidores ou adicionar outros servidores ao grupo usando o **do Active Directory**, **DNS**, ou **importar**guias. Para obter mais informações sobre como usar essas guias, consulte [adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md) neste guia.

4.  Após concluir a adição de servidores ao grupo, clique em **OK**. O novo grupo é exibido no painel de navegação do Gerenciador de servidores sob o **todos os servidores** grupo.

#### <a name="to-edit-an-existing-server-group"></a>Para editar um grupo de servidores existente

1.  Siga um destes procedimentos.

    -   No painel de navegação do Gerenciador do servidor, um grupo de servidores com o botão direito e, em seguida, clique em **Editar grupo de servidores**.

    -   Na home page do grupo de servidores, abra o **tarefas** menu a **servidores** lado a lado e, em seguida, clique em **Editar grupo de servidores**.

2.  alterar o nome do grupo, ou adicionar ou remover servidores do grupo.

    > [!NOTE]
    > remoção de servidores de um grupo de servidores não remove os servidores do Gerenciador de servidores. Os servidores removidos de um grupo permanecem no grupo **Todos os Servidores** , no pool de servidores.

3.  Após concluir as alterações no grupo, clique em **OK**.

#### <a name="to-delete-an-existing-server-group"></a>Para excluir um grupo de servidores existente

1.  Siga um destes procedimentos.

    -   No painel de navegação do Gerenciador do servidor, um grupo de servidores com o botão direito e, em seguida, clique em **excluir grupo de servidores**.

    -   Na home page do grupo de servidores, abra o **tarefas** menu a **servidores** lado a lado e, em seguida, clique em **excluir grupo de servidores**.

2.  Quando for exibida uma mensagem perguntando se você deseja excluir o grupo de servidores, clique em **Sim**.

    > [!NOTE]
    > Excluir um grupo de servidores não remove os servidores do Gerenciador de servidores. Os servidores que estavam em um grupo excluído permanecem no grupo **Todos os Servidores** , no pool de servidores.

3.  Após concluir as alterações no grupo, clique em **OK**.

## <a name="see-also"></a>Consulte também
[Adicionar servidores ao Gerenciador do servidor](add-servers-to-server-manager.md)
[Gerenciador do servidor](server-manager.md)



