---
title: Criar uma função de usuário para controle de acesso
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ipam
ms.topic: article
ms.assetid: ae6a42db-a104-401b-a8e6-b85c47d30b46
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 90ba50189b0f42f1f581032b7dc8b52b8c3fca4d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80814759"
---
# <a name="create-a-user-role-for-access-control"></a>Criar uma função de usuário para controle de acesso

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para criar uma nova função de usuário de controle de acesso no console do cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
> [!NOTE]  
> Depois de criar uma função, você pode criar uma política de acesso para atribuir a função a um usuário ou grupo de Active Directory específico. Para obter mais informações, consulte [criar uma política de acesso](../../technologies/ipam/Create-an-Access-Policy.md).  
  
### <a name="to-create-a-role"></a>Para criar uma função  
  
1.  Em Gerenciador do Servidor, clique em **IPAM**. O console do cliente IPAM é exibido.  
  
2.  No painel de navegação, clique em **controle de acesso**e, no painel de navegação inferior, clique em **funções**.  
  
    ![Funções de controle de acesso](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_01.jpg)  
  
3.  Clique com o botão direito do mouse em **funções**e clique em **Adicionar função de usuário**.  
  
    ![Adicionar função de usuário](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_02.jpg)  
  
4.  A caixa de diálogo **Adicionar ou Editar função** é aberta. Em **nome**, digite um nome para a função que torna a função de função clara. Por exemplo, se você quiser criar uma função que permita que os administradores gerenciem registros de recurso SRV DNS, você pode nomear a função **IPAMSrv**. Se necessário, role para baixo em **operações** para localizar o tipo de operações que você deseja definir para a função. Para este exemplo, role para baixo até **as operações de gerenciamento de registro de recurso de DNS**.  
  
    ![Operações de gerenciamento de registro de recurso DNS](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_03.jpg)  
  
5.  Expanda **operações de gerenciamento de registro de recurso DNS**e localize **operações de registro SRV**.  
  
    ![Operações de registro SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_04.jpg)  
  
6.  Expanda e selecione **as operações de registro SRV**e clique em **OK**.  
  
    ![Selecionar operações de registro SRV](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_05.jpg)  
  
7.  No console do cliente IPAM, clique na função que você acabou de criar. Na **exibição de detalhes,** as operações permitidas para a função são exibidas.  
  
    ![Detalhes da nova função](../../media/Create-a-User-Role-for-Access-Control/ipam_CreateUserRole_06.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso baseado em função](Role-based-Access-Control.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


