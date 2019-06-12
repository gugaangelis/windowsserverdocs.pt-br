---
title: Personalizar o painel de navegação do Centro Administrativo do Active Directory
ms.prod: windows-server-threshold
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
ms.openlocfilehash: a4cab0246226cf22a1b7212b832a902783952407
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446499"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalizar o painel de navegação do Centro Administrativo do Active Directory

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

  Você pode procurar por meio do painel de navegação da Central Administrativa do Active Directory usando o modo de exibição de árvore, que é semelhante à árvore de console em computadores e usuários do Active Directory, ou por meio da exibição de lista.

 Se você usar o modo de exibição de árvore ou a exibição de lista, você pode personalizar seu painel de navegação da Central Administrativa do Active Directory a qualquer momento adicionando vários contêineres do domínio local ou de qualquer domínio externo \(ou seja, um domínio diferente do domínio local que tem uma relação de confiança estabelecida com o domínio local\) ao painel de navegação como nós separados. Personalizando o painel de navegação da Central Administrativa do Active Directory pode fornecer acesso mais rápido aos objetos do Active Directory. Para obter mais informações, consulte [gerenciar diferentes domínios no Centro Administrativo do Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 Além disso, para personalizar ainda mais o painel de navegação, é possível renomear ou remover esses nós adicionados manualmente, criar duplicatas desses nós ou movê-los para cima ou para baixo no painel.

> [!NOTE]
>  Não é possível personalizar o nó de domínio local padrão.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Para personalizar o painel de navegação da Central Administrativa do Active Directory

1. No painel de navegação da Central Administrativa do Active Directory, com o botão direito\-clique no nó que você deseja modificar. É possível modificar a posição ou o nome do nó ou criar uma duplicata do mesmo.

2. Clique em um dos seguintes comandos:

   -   **Renomear**

   -   **Criar nó duplicado**

   -   **Remover**

   -   **Mover para cima**

   -   **Mover para baixo**

   Ao usar o modo de exibição de lista, você pode tirar proveito dos mais usados recentemente \(MRU\) lista. A lista MRU é exibida automaticamente sob um nó de navegação quando você visita pelo menos um contêiner neste nó de navegação. Você também pode exibir a lista MRU atual expandindo a barra estrutural na parte superior da janela Central Administrativa do Active Directory. A lista MRU sempre contém os três últimos contêineres que você visitou em um determinado nó de navegação. Toda vez que você selecionar um contêiner específico, ele será adicionado ao topo da lista MRU e o último contêiner da lista será removido.

  

