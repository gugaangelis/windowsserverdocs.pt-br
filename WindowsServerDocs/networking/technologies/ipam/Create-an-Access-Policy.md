---
title: Criar uma política de acesso
description: Este tópico faz parte do guia de gerenciamento do gerenciamento de endereço IP (IPAM) no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ipam
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c8a97cd145a695bc8755f9111291e5c8bba2e572
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881897"
---
# <a name="create-an-access-policy"></a>Criar uma política de acesso

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para criar uma política de acesso no console de cliente IPAM.  
  
A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
> [!NOTE]  
> Você pode criar uma política de acesso para um usuário específico ou para um grupo de usuários no Active Directory. Quando você cria uma política de acesso, você deve selecionar uma função interna do IPAM ou uma função personalizada que você criou. Para obter mais informações sobre funções personalizadas, consulte [criar uma função de usuário para o controle de acesso](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).  
  
### <a name="to-create-an-access-policy"></a>Para criar uma política de acesso  
  
1.  No Gerenciador do servidor, clique em **IPAM**. Console de cliente IPAM é exibida.  
  
2.  No painel de navegação, clique em **controle de acesso**. No painel de navegação inferior, clique com botão direito **políticas de acesso**e, em seguida, clique em **adicionar política de acesso**.  
  
    ![Adicionar política de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  O **adicionar política de acesso** caixa de diálogo é aberta. Na **as configurações de usuário**, clique em **Add**.  
  
    ![Adicionar política de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  O **Selecionar usuário ou grupo** caixa de diálogo é aberta. Clique em **locais**.  
  
    ![Locais de usuário ou grupo](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  O **locais** caixa de diálogo é aberta. Navegue até o local que contém a conta de usuário, selecione o local e, em seguida, clique em **Okey**. O **locais** caixa de diálogo é fechada.  
  
    ![Selecionar local](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  No **Selecionar usuário ou grupo** na caixa **insira o nome do objeto para selecionar**, digite o nome da conta de usuário para o qual você deseja criar uma política de acesso. Clique em **OK**.  
  
7.  Na **adicionar política de acesso**, na **configurações do usuário**, **alias do usuário** agora contém a conta de usuário aos quais a política se aplica. Na **as configurações de acesso**, clique em **New**.  
  
    ![Nova configuração de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  Na **adicionar política de acesso**, **configurações de acesso** muda para **nova configuração**.  
  
    ![Nome da caixa de diálogo Alterar a nova configuração](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. Clique em **Selecionar função** para expandir a lista de funções. Selecione uma das funções internas ou, se você tiver criado as novas funções, selecione uma das funções que você criou. Por exemplo, se você tiver criado a função IPAMSrv para aplicar ao usuário, clique em **IPAMSrv**.  
  
    ![Selecionar função](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. Clique em **Adicionar configuração**.  
  
    ![Adicionar nova configuração](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. A função é adicionada à política de acesso. Para criar políticas de acesso adicionais, clique em **aplicar**e, em seguida, repita essas etapas para cada política que você deseja criar. Se você não deseja criar diretivas adicionais, clique em **Okey**.  
  
    ![Clique em Aplicar ou Okey](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. No painel de exibição do console de cliente IPAM, verifique se que a nova política de acesso é criada.  
  
    ![Exibir a nova política de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso baseado em função](Role-based-Access-Control.md)  
[Gerenciar o IPAM](Manage-IPAM.md)  
  


