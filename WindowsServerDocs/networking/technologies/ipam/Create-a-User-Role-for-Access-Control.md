---
title: Criar uma função de usuário para controle de acesso
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3798b074a0ca7e20602da7986fe6b54e81da5495
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67284093"
---
# <a name="create-a-user-role-for-access-control"></a>Criar uma função de usuário para controle de acesso

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para criar uma nova função de usuário do controle de acesso no console de cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
> [!NOTE]  
> Depois de criar uma função, você pode criar uma política de acesso para atribuir a função a um usuário específico ou grupo do Active Directory. Para obter mais informações, consulte [criar uma política de acesso](../../technologies/ipam/Create-an-Access-Policy.md).  
  
### <a name="to-create-a-role"></a>Para criar uma função  
  
1.  No Gerenciador do servidor, clique em **IPAM**. Console de cliente IPAM é exibida.  
  
2.  No painel de navegação, clique em **controle de acesso**e no painel de navegação inferior, clique em **funções**.  
  
    ![Funções de controle de acesso](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  Clique com botão direito **funções**e, em seguida, clique em **Adicionar função de usuário**.  
  
    ![Adicionar função de usuário](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  O **adicionar ou Editar função** caixa de diálogo é aberta. Na **nome**, digite um nome para a função que faz com que a função função Limpar. Por exemplo, se você quiser criar uma função que permite aos administradores gerenciar registros de recurso SRV de DNS, você pode nomear a função **IPAMSrv**. Se necessário, role para baixo na **operações** para localizar o tipo das operações que você deseja definir para a função. Neste exemplo, role para baixo até **operações de gerenciamento de registros de recursos DNS**.  
  
    ![Operações de gerenciamento de registros de recursos DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  Expandir **operações de gerenciamento de registros de recursos DNS**e, em seguida, localize **operações de registro de SRV**.  
  
    ![Operações de registro SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  Expanda e selecione **operações de registro de SRV**e, em seguida, clique em **Okey**.  
  
    ![Selecionar operações de registro de SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  No console do cliente IPAM, clique na função que você acabou de criar. Na **exibição de detalhes,** as operações permitidas para a função são exibidas.  
  
    ![Detalhes da nova função](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso baseado em função](Role-based-Access-Control.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


