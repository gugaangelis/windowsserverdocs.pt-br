---
title: Personalizar o painel de navegação Centro Administrativo do Active Directory
ms.prod: windows-server
description: Segurança do Windows Server
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 63038014207acd3846cb8db20c7836718615df51
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403754"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalizar o painel de navegação Centro Administrativo do Active Directory

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

  Você pode navegar pelo painel de navegação Centro Administrativo do Active Directory usando o modo de exibição de árvore, que é semelhante à árvore de console Active Directory usuários e computadores, ou usando o modo de exibição de lista.

 Se você usar a exibição de árvore ou a exibição de lista, poderá personalizar seu Centro Administrativo do Active Directory painel de navegação a qualquer momento adicionando vários contêineres do domínio local ou de qualquer domínio externo \(that for, um domínio diferente do domínio local que tem uma relação de confiança estabelecida com o domínio local @ no__t-1 para o painel de navegação como nós separados. Personalizar o painel de navegação Centro Administrativo do Active Directory pode fornecer acesso mais rápido a objetos Active Directory. Para obter mais informações, consulte [gerenciar domínios diferentes no centro administrativo do Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 Além disso, para personalizar ainda mais o painel de navegação, é possível renomear ou remover esses nós adicionados manualmente, criar duplicatas desses nós ou movê-los para cima ou para baixo no painel.

> [!NOTE]
>  Não é possível personalizar o nó de domínio local padrão.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Para personalizar o painel de navegação Centro Administrativo do Active Directory

1. No painel de navegação Centro Administrativo do Active Directory, clique com o botão direito @ no__t-0click no nó que você deseja modificar. É possível modificar a posição ou o nome do nó ou criar uma duplicata do mesmo.

2. Clique em um dos seguintes comandos:

   -   **Nome**

   -   **Criar nó duplicado**

   -   **Remover**

   -   **Mover para cima**

   -   **Mover para baixo**

   Usando o modo de exibição de lista, você pode aproveitar a lista de @no__t 0MRU @ no__t-1 usada mais recentemente. A lista MRU aparece automaticamente em um nó de navegação quando você visita pelo menos um contêiner neste nó de navegação. Você também pode exibir a lista MRU atual expandindo a barra de navegação estrutural na parte superior da janela de Centro Administrativo do Active Directory. A lista MRU sempre contém os três últimos contêineres que você visitou em um nó de navegação específico. Toda vez que você selecionar um contêiner específico, ele será adicionado ao topo da lista MRU e o último contêiner da lista será removido.

  

