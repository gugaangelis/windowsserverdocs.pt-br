---
title: Criar uma política de acesso
description: Este tópico faz parte do guia de gerenciamento do IPAM (gerenciamento de endereços IP) no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: 854bd064-2f86-4678-a940-a04b3e48ae10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d454bef1a6bc15b5f8a6f2e6f78773785ad5af69
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87966563"
---
# <a name="create-an-access-policy"></a>Criar uma política de acesso

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para criar uma política de acesso no console do cliente IPAM.

A associação em **Administradores**, ou equivalente, é o requisito mínimo para executar este procedimento.

> [!NOTE]
> Você pode criar uma política de acesso para um usuário específico ou para um grupo de usuários no Active Directory. Ao criar uma política de acesso, você deve selecionar uma função interna do IPAM ou uma função personalizada que você criou. Para obter mais informações sobre as funções personalizadas, consulte [criar uma função de usuário para o controle de acesso](../../technologies/ipam/Create-a-User-Role-for-Access-Control.md).

### <a name="to-create-an-access-policy"></a>Para criar uma política de acesso

1.  Em Gerenciador do Servidor, clique em **IPAM**. O console do cliente IPAM é exibido.

2.  No painel de navegação, clique em **controle de acesso**. No painel de navegação inferior, clique com o botão direito do mouse em **políticas de acesso**e clique em **Adicionar política de acesso**.

    ![Adicionar política de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_01.jpg)

3.  A caixa de diálogo **Adicionar política de acesso** é aberta. Em **configurações do usuário**, clique em **Adicionar**.

    ![Adicionar política de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_02.jpg)

4.  A caixa de diálogo **Selecionar usuário ou grupo** é aberta. Clique em **Locais**.

    ![Locais de usuário ou grupo](../../media/Create-an-Access-Policy/ipam_CreateAP_03.jpg)

5.  A caixa de diálogo **locais** é aberta. Navegue até o local que contém a conta de usuário, selecione o local e clique em **OK**. A caixa de diálogo **locais** é fechada.

    ![Selecionar local](../../media/Create-an-Access-Policy/ipam_CreateAP_04.jpg)

6.  Na caixa de diálogo **Selecionar usuário ou grupo** , em **digite o nome do objeto a ser selecionado**, digite o nome da conta de usuário para a qual você deseja criar uma política de acesso. Clique em **OK**.

7.  Em **Adicionar política de acesso**, em **configurações de usuário**, o alias de **usuário** agora contém a conta de usuário à qual a política se aplica. Em **configurações de acesso**, clique em **novo**.

    ![Nova configuração de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_05.jpg)

8.  Em **Adicionar política de acesso**, **configurações de acesso** altera para **nova configuração**.

    ![Nome da caixa de diálogo Alterar para nova configuração](../../media/Create-an-Access-Policy/ipam_CreateAP_06.jpg)

9. Clique em **selecionar função** para expandir a lista de funções. Selecione uma das funções internas ou, se você tiver criado novas funções, selecione uma das funções que você criou. Por exemplo, se você criou a função IPAMSrv para aplicar ao usuário, clique em **IPAMSrv**.

    ![Selecione a função](../../media/Create-an-Access-Policy/ipam_CreateAP_07.jpg)

10. Clique em **Adicionar configuração**.

    ![Adicionar nova configuração](../../media/Create-an-Access-Policy/ipam_CreateAP_08.jpg)

11. A função é adicionada à política de acesso. Para criar políticas de acesso adicionais, clique em **aplicar**e repita essas etapas para cada política que você deseja criar. Se você não quiser criar políticas adicionais, clique em **OK**.

    ![Clique em aplicar ou em OK](../../media/Create-an-Access-Policy/ipam_CreateAP_09.jpg)

12. No painel de exibição do console do cliente IPAM, verifique se a nova política de acesso foi criada.

    ![Exibir a nova política de acesso](../../media/Create-an-Access-Policy/ipam_CreateAP_09a.jpg)

## <a name="see-also"></a>Consulte Também
Controle de acesso [baseado em função](Role-based-Access-Control.md) 
 [Gerenciar IPAM](Manage-IPAM.md)



