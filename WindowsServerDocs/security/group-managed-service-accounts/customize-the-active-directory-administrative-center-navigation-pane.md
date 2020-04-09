---
title: Personalizar o painel de navegação Centro Administrativo do Active Directory
ms.prod: windows-server
description: Segurança do Windows Server
ms.assetid: c9933d16-e153-435a-b5b7-3e594db42d5c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 0053fc4e4f3e22a8fd98e242e38fc7c4e2002867
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856999"
---
# <a name="customize-the-active-directory-administrative-center-navigation-pane"></a>Personalizar o painel de navegação Centro Administrativo do Active Directory

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

  Você pode navegar pelo painel de navegação Centro Administrativo do Active Directory usando o modo de exibição de árvore, que é semelhante à árvore de console Active Directory usuários e computadores, ou usando o modo de exibição de lista.

 Se você usar a exibição de árvore ou a exibição de lista, poderá personalizar seu Centro Administrativo do Active Directory painel de navegação a qualquer momento adicionando vários contêineres do domínio local ou de qualquer domínio externo \(ou seja, um domínio diferente do domínio local que tenha uma relação de confiança estabelecida com o domínio local\) ao painel de navegação como nós separados. Personalizar o painel de navegação Centro Administrativo do Active Directory pode fornecer acesso mais rápido a objetos Active Directory. Para obter mais informações, consulte [gerenciar domínios diferentes no centro administrativo do Active Directory](manage-different-domains-in-active-directory-administrative-center.md).

 Além disso, para personalizar ainda mais o painel de navegação, é possível renomear ou remover esses nós adicionados manualmente, criar duplicatas desses nós ou movê-los para cima ou para baixo no painel.

> [!NOTE]
>  Não é possível personalizar o nó de domínio local padrão.

### <a name="to-customize-the-active-directory-administrative-center-navigation-pane"></a>Para personalizar o painel de navegação Centro Administrativo do Active Directory

1. No painel de navegação Centro Administrativo do Active Directory, à direita\-clique no nó que você deseja modificar. É possível modificar a posição ou o nome do nó ou criar uma duplicata do mesmo.

2. Clique em um dos seguintes comandos:

   -   **Nome**

   -   **Criar nó duplicado**

   -   **Remover**

   -   **Mover para cima**

   -   **Mover para baixo**

   Usando o modo de exibição de lista, você pode tirar proveito da lista de\) MRU \(mais usada recentemente. A lista MRU aparece automaticamente em um nó de navegação quando você visita pelo menos um contêiner neste nó de navegação. Você também pode exibir a lista MRU atual expandindo a barra de navegação estrutural na parte superior da janela de Centro Administrativo do Active Directory. A lista MRU sempre contém os três últimos contêineres que você visitou em um nó de navegação específico. Toda vez que você selecionar um contêiner específico, ele será adicionado ao topo da lista MRU e o último contêiner da lista será removido.

  

