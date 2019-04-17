---
title: Criar uma política de acesso
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
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ac8229952c7e038f9af8fc4f9287b1821db887ac
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="create-an-access-policy"></a>Criar uma política de acesso

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para criar uma política de acesso no console do cliente IPAM.  
  
A associação ao grupo **administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.  
  
> [!NOTE]  
> Você pode criar uma política de acesso de um usuário específico ou para um grupo de usuários no Active Directory. Quando você cria uma política de acesso, você deve selecionar uma função IPAM interna ou uma função personalizada que você criou. Para saber mais sobre funções personalizadas, veja [criar uma função de usuário para o controle de acesso](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).  
  
### <a name="to-create-an-access-policy"></a>Para criar uma política de acesso  
  
1.  No Gerenciador do servidor, clique em **IPAM**. O console de cliente IPAM aparece.  
  
2.  No painel de navegação, clique em **o controle de acesso**. No painel de navegação inferior, clique com botão direito **políticas de acesso**e clique em **adicionar política de acesso**.  
  
    ![Adicionar política de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)  
  
3.  O **adicionar política de acesso** caixa de diálogo é aberta. Em **as configurações do usuário**, clique em **adicionar**.  
  
    ![Adicionar política de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)  
  
4.  O **Selecionar usuário ou grupo** caixa de diálogo é aberta. Clique em **locais**.  
  
    ![Locais de usuário ou grupo](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)  
  
5.  O **locais** caixa de diálogo é aberta. Navegue até o local que contém a conta de usuário, selecione o local e, em seguida, clique em **Okey**. O **locais** caixa de diálogo fecha.  
  
    ![Selecione local](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)  
  
6.  No **Selecionar usuário ou grupo** na caixa **insira o nome do objeto para selecionar**, digite o nome da conta de usuário para o qual você deseja criar uma política de acesso. Clique em **Okey**.  
  
7.  Em **adicionar política de acesso**, na **as configurações do usuário**, **alias de usuário** agora contém a conta de usuário ao qual a política se aplica. Em **as configurações de acesso**, clique em **nova**.  
  
    ![Nova configuração de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)  
  
8.  Em **adicionar política de acesso**, **as configurações de acesso** muda para **nova configuração**.  
  
    ![Nome da caixa de diálogo Alterar a nova configuração](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)  
  
9. Clique em **função Select** para expandir a lista de funções. Selecione uma das funções internas ou, se você criou novas funções, selecione uma das funções que você criou. Por exemplo, se você criou a função IPAMSrv para aplicar ao usuário, clique em **IPAMSrv**.  
  
    ![Selecione a função](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)  
  
10. Clique em **Adicionar configuração**.  
  
    ![Adicionar nova configuração](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)  
  
11. A função é adicionada à política de acesso. Para criar políticas de acesso adicionais, clique em **aplicar**e, em seguida, repita essas etapas para cada política que você deseja criar. Se você não quiser criar políticas adicionais, clique em **Okey**.  
  
    ![Clique em Aplicar ou Okey](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)  
  
12. No painel de exibição de console IPAM cliente, verifique se que a nova política de acesso é criada.  
  
    ![Exibir a nova política de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)  
  
## <a name="see-also"></a>Consulte também  
[Controle de acesso baseado em função](Role-based-Access-Control.md)  
[Gerenciar IPAM](Manage-IPAM.md)  
  


