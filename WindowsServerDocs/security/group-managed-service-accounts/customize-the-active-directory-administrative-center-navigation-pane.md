---
title: Personalizar o painel de navegação Centro Administrativo do Active Directory
description: Segurança do Windows Server
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: 34a15b768d3143d7083ab62bb9aa537a2c8bdeb4
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89639792"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalizar o painel de navegação Centro Administrativo do Active Directory

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

  Você pode navegar pelo painel de navegação Centro Administrativo do Active Directory usando o modo de exibição de árvore, que é semelhante à árvore de console Active Directory usuários e computadores, ou usando o modo de exibição de lista.

 Se você usar a exibição de árvore ou a exibição de lista, poderá personalizar seu Centro Administrativo do Active Directory painel de navegação a qualquer momento adicionando vários contêineres do domínio local ou de qualquer domínio externo \( que seja, um domínio diferente do domínio local que tenha uma relação de confiança estabelecida com o domínio local \) para o painel de navegação como nós separados. Personalizar o painel de navegação Centro Administrativo do Active Directory pode fornecer acesso mais rápido a objetos Active Directory. Para obter mais informações, consulte [gerenciar domínios diferentes no centro administrativo do Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 Além disso, para personalizar ainda mais o painel de navegação, você pode renomear ou remover os nós adicionados manualmente, criar duplicatas deles ou movê-los para cima ou para baixo no painel de navegação.

> [!NOTE]
>  Não é possível personalizar o nó de domínio local padrão.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Para personalizar o painel de navegação do Centro Administrativo do Active Directory

1. No painel de navegação Centro Administrativo do Active Directory, \- clique com o botão direito do mouse no nó que você deseja modificar. É possível modificar a posição ou o nome do nó, ou você pode criar uma duplicata dele.

2. Clique em um dos seguintes comandos:

   -   **Renomear**

   -   **Criar nó duplicado**

   -   **Remover**

   -   **Mover para cima**

   -   **Mover para baixo**

   Usando o modo de exibição de lista, você pode aproveitar a lista MRU usada mais recentemente \( \) . A lista MRU aparece automaticamente em um nó de navegação quando você visita pelo menos um contêiner neste nó de navegação. Você também pode exibir a lista MRU atual expandindo a barra de navegação estrutural na parte superior da janela de Centro Administrativo do Active Directory. A lista MRU sempre contém os três últimos contêineres que você visitou em um nó de navegação específico. Cada vez que você selecionar determinado contêiner, ele será adicionado ao início da lista MRU, e o último contêiner da lista será removido.



