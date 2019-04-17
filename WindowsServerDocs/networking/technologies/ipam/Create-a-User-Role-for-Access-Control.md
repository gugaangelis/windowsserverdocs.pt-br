---
title: Crie uma função de usuário para o controle de acesso
description: Este tópico faz parte do guia de gerenciamento de gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: pashort
author: shortpatti
ms.openlocfilehash: fa0ed71d399ad638a648946952fe170d93f69ceb
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="create-a-user-role-for-access-control"></a>Crie uma função de usuário para o controle de acesso

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para criar uma nova função de usuário de controle de acesso no console do cliente IPAM.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
> [!NOTE]  
> Depois de criar uma função, você pode criar uma política de acesso para atribuir a função para um determinado usuário ou grupo do Active Directory. Para obter mais informações, consulte [criar uma política de acesso](../../technologies/ipam/Create-an-Access-Policy.md).  
  
### <a name="to-create-a-role"></a>Para criar uma função  
  
1.  No Gerenciador do servidor, clique em **IPAM**. O console de cliente IPAM aparece.  
  
2.  No painel de navegação, clique em **o controle de acesso**e no painel de navegação inferior, clique em **funções**.  
  
    ![Funções de controle de acesso](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  Clique com botão direito **funções**e clique em **adicionar a função de usuário**.  
  
    ![Adicione a função de usuário](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  O **adicionar ou Editar função** caixa de diálogo é aberta. Em **nome**, digite um nome para a função que faz com que a função Limpar. Por exemplo, se você quiser criar uma função que permite que os administradores gerenciar registros de recurso SRV do DNS, você pode chamar a função **IPAMSrv**. Se necessário, role para baixo em **operações** para localizar o tipo de operações que você deseja definir para a função. Para este exemplo, role para baixo até **operações de gerenciamento de registros de recurso DNS**.  
  
    ![Operações de gerenciamento de registros de recurso do DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  Expanda **operações de gerenciamento de registros de recurso DNS**e, em seguida, localizar **operações de registros SRV**.  
  
    ![Operações de registro SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  Expanda e selecione **operações de registros SRV**e clique em **Okey**.  
  
    ![Selecione operações de registros SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  No console do cliente IPAM, clique na função que você acabou de criar. Em **modo de exibição de detalhes,** as operações permitidas para a função são exibidas.  
  
    ![Novos detalhes da função](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso baseado em função](Role-based-Access-Control.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


