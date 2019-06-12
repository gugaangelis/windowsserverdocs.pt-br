---
title: Gerenciar diferentes domínios no Centro Administrativo do Active Directory
ms.prod: windows-server-threshold
description: Segurança do Windows Server
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: bd5650724272422d09e87b7eecf10f825b00fabf
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66447045"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Gerenciar diferentes domínios no Centro Administrativo do Active Directory

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

  Quando você abre administrativas do Active Directory, o domínio ao qual você fez logon neste computador \(domínio local\) é exibida no painel de navegação da Central Administrativa do Active Directory \(dopainelàesquerda\). Dependendo dos direitos do seu conjunto atual de credenciais de logon, você pode exibir ou gerenciar objetos do Active Directory no domínio local.

 Você também pode usar o mesmo conjunto de credenciais de logon e a mesma instância da Central Administrativa do Active Directory para exibir ou gerenciar objetos do Active Directory em qualquer outro domínio na mesma floresta ou domínio em outra floresta que tenha uma confiança estabelecida com o local domínio. Ambos os uma\-dois e relações de confiança de maneira\-relações de confiança de maneira têm suporte.

> [!NOTE]
>  Se não houver um\-de confiança mútua entre o domínio A e o domínio B por meio do qual os usuários no domínio A podem acessar recursos no domínio B, mas os usuários no domínio B não podem acessar recursos no domínio A, se você estiver executando o Centro Administrativo do Active Directory no computador em que o domínio A é o seu domínio local, você pode se conectar ao domínio B com o conjunto de credenciais de logon e na mesma instância da Central Administrativa do Active Directory atual. Mas se você estiver executando o Centro Administrativo no computador em que o domínio B é o domínio local, você não pode se conectar ao domínio A com o mesmo conjunto de credenciais na mesma instância do Centro Administrativo do Active Directory.

 Nenhum requisito mínimo de associação a grupo é necessário para a conclusão deste procedimento.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012: Para gerenciar um domínio externo na instância selecionada da Central Administrativa do Active Directory usando o conjunto atual de credenciais de logon

1.  Para abrir o Centro Administrativo do Active Directory, no **Gerenciador de servidores**, clique em **ferramentas**e, em seguida, clique em **Central Administrativa do Active Directory**.

    > [!NOTE]
    >  Outra maneira de abrir a Central Administrativa do Active Directory é clicar **inicie**e, em seguida, digite **dsac.exe**.

2.  Para abrir **adicionar nós de navegação**, clique em **gerenciar**, em seguida, clique em **adicionar nós de navegação** conforme mostrado na ilustração a seguir.

     ![Captura de tela mostrando * * adicionar navegação nós * * interface de usuário](media/ADDS_ADACAddNavNode.gif)

3.  Na **adicionar nós de navegação**, clique em **conectar a outros domínios** conforme mostrado na ilustração a seguir.

     ![Captura de tela mostrando * * adicionar navegação nós * * interface de usuário](media/ADDS_ADACConnectToDomain.gif)

4.  Na **conectem**, digite o nome do domínio externo que você deseja gerenciar \(por exemplo, **contoso.com**\)e, em seguida, clique em **Okey**.

5.  Quando você está conectado com êxito ao domínio externo, percorra as colunas na **adicionar nós de navegação** janela, selecione um ou mais contêineres para adicionar ao seu painel de navegação da Central Administrativa do Active Directory, e em seguida, clique em **Okey**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2: Para gerenciar um domínio externo na instância selecionada da Central Administrativa do Active Directory usando o conjunto atual de credenciais de logon

1. Para abrir a Central Administrativa do Active Directory, clique em **inicie**, clique em **ferramentas administrativas**e, em seguida, clique em **Central Administrativa do Active Directory**.

   > [!NOTE]
   >  Outra maneira de abrir a Central Administrativa do Active Directory é clicar **inicie**, clique em **execute**e, em seguida, digite **dsac.exe**.

2. Para abrir **adicionar nós de navegação**, na parte superior da janela Central Administrativa do Active Directory, clique em **adicionar nós de navegação** conforme mostrado na ilustração a seguir.

    ![Captura de tela mostrando * * adicionar navegação nós * * interface de usuário](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  Outra maneira de abrir **adicionar nós de navegação** é para a direita\-clique em qualquer lugar em um espaço vazio no painel de navegação da Central Administrativa do Active Directory e, em seguida, clique em **adicionar nós de navegação**.

3. Na **adicionar nós de navegação**, clique em **conectar a outros domínios** conforme mostrado na ilustração a seguir.

    ![Captura de tela mostrando * * adicionar navegação nós * * * * conectar-se a outros domínios * * da interface do usuário](media/add_nav_nodes.gif)

4. Na **conectem**, digite o nome do domínio externo que você deseja gerenciar \(por exemplo, **contoso.com**\)e, em seguida, clique em **Okey**.

5. Quando você está conectado com êxito ao domínio externo, percorra as colunas na **adicionar nós de navegação** janela, selecione um ou mais contêineres para adicionar ao seu painel de navegação da Central Administrativa do Active Directory, e em seguida, clique em **Okey**.

   Para obter mais informações sobre como personalizar o painel de navegação da Central Administrativa do Active Directory, consulte [personalizar o painel do Active Directory administrativas Center navegação](customize-the-active-directory-administrative-center-navigation-pane.md).

   Você também pode abrir a Central Administrativa do Active Directory usando um conjunto de credenciais de logon que seja diferente do conjunto atual de credenciais de logon. O comando no procedimento a seguir pode ser útil se você estiver conectado ao computador que está executando a Central Administrativa do Active Directory com credenciais de usuário normal, mas você deseja usar a Central Administrativa do Active Directory nesse computador para gerenciar seu domínio local como administrador. \(Este comando também pode ser útil se você quiser usar a Central Administrativa do Active Directory para gerenciar remotamente um domínio externo que é diferente do seu domínio local com um conjunto de credenciais é diferente do conjunto atual de credenciais de logon. No entanto, o domínio externo deve ter uma confiança estabelecida com o domínio local.\)

   Nenhum requisito mínimo de associação a grupo é necessário para a conclusão deste procedimento.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Para gerenciar um domínio usando credenciais de logon diferentes do conjunto de credenciais de logon atual

1.  Para abrir o Centro Administrativo do Active Directory, no prompt de comando, digite o seguinte comando e pressione ENTER:

     `runas /user:<domain\user> dsac`

     Em que `<domain\user>` é o conjunto de credenciais que você deseja abrir o Centro Administrativo do Active Directory com e `dsac` é o nome de arquivo executável da Central Administrativa do Active Directory \(Dsac.exe\).

     Por exemplo, digite o seguinte comando e pressione ENTER:

     `runas /user:contoso\administrator dsac`

2.  Quando a Central Administrativa do Active Directory é aberto, procure por meio do painel de navegação para exibir ou gerenciar o domínio do Active Directory.

  

