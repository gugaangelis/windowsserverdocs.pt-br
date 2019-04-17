---
title: "Personalizar o painel de navegação do Centro Administrativo do Active Directory"
ms.prod: windows-server-threshold
description: "Segurança do Windows Server"
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: e7b1128d93912f724225905bedd38131f8aab0b2
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalizar o painel de navegação do Centro Administrativo do Active Directory

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

  Você pode navegar através do painel de navegação do Centro Administrativo do Active Directory usando o modo de exibição de árvore, que é semelhante à árvore de console Active Directory usuários e computadores, ou usando o modo de exibição de lista.

 Se você usar o modo de exibição de árvore ou o modo de exibição de lista, você pode personalizar o painel de navegação do Centro Administrativo do Active Directory a qualquer momento adicionando vários contêineres de domínio local ou em qualquer domínio externo \ (ou seja, um domínio diferente do local que tenha um estabelece confiança com o local domain\) para o painel de navegação como nós separados. Personalizando o painel de navegação do Centro Administrativo do Active Directory pode fornecer acesso rápido aos objetos do Active Directory. Para obter mais informações, consulte [gerenciar domínios diferentes no Centro Administrativo do Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 Além disso, para personalizar ainda mais o painel de navegação, você pode renomear ou remover esses nós do painel de navegação adicionado manualmente, criar duplicatas de nós ou movê-los para cima ou para baixo no painel de navegação.

> [!NOTE]
>  Você não pode personalizar o nó de domínio local padrão.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Para personalizar o painel de navegação do Centro Administrativo do Active Directory

1.  No painel de navegação Centro Administrativo do Active Directory, clique right\ no nó que você deseja modificar. Você pode modificar a posição ou o nome do nó, ou você pode criar uma cópia dela.

2.  Clique em um dos seguintes comandos:

    -   **Renomear**

    -   **Criar nó duplicado**

    -   **Remover**

    -   **Mover para cima**

    -   **Mover para baixo**

 Usando o modo de exibição de lista, você pode tirar proveito da lista \(MRU\) usados recentemente. A lista MRU aparece automaticamente em um nó de navegação quando você visita pelo menos um contêiner nesse nó de navegação. Você também pode exibir a lista MRU atual expandindo a barra de navegação estrutural na parte superior da janela do Centro Administrativo do Active Directory. A lista MRU sempre contém os três últimos contêineres que você visitou em um nó de navegação em particular. Toda vez que você selecione um determinado contêiner, esse contêiner é adicionado à parte superior da lista MRU e o último contêiner na lista MRU é removido.

  

