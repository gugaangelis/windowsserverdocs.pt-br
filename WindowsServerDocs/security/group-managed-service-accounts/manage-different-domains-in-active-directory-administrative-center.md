---
title: "Gerenciar domínios diferentes no Centro Administrativo do Active Directory"
ms.prod: windows-server-threshold
description: "Segurança do Windows Server"
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 5f253bd4952d8a347e97eafdb38d86fa98024b8d
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Gerenciar domínios diferentes no Centro Administrativo do Active Directory

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

  Quando você abrir administrativas do Active Directory, o domínio que você fez logon neste computador \ (o local domain\) é exibido no painel de navegação Centro Administrativo do Active Directory \(the left pane\). Dependendo dos direitos de seu conjunto atual de credenciais de logon, você pode exibir ou gerenciar objetos do Active Directory neste domínio local.

 Você também pode usar o mesmo conjunto de credenciais de logon e a mesma instância do Centro Administrativo do Active Directory para visualizar ou gerenciar objetos do Active Directory em um domínio em outra floresta que tem uma relação de confiança estabelecida com o local ou algum outro domínio na floresta do mesmo domínio. Relações de confiança de maneira one\ e relações de confiança de maneira two\ têm suporte.

> [!NOTE]
>  Se houver uma relação de confiança entre o domínio A e B de domínio por meio de one\ vias quais usuários em can de domínio A acessam os recursos no domínio B, mas os usuários no domínio B não podem acessar recursos em um domínio, se você estiver executando Centro Administrativo do Active Directory no computador onde está a um domínio  seu domínio local, você pode se conectar ao domínio B com o conjunto de credenciais de logon e na mesma instância do Centro Administrativo do Active Directory atual. Mas se você estiver executando o Centro Administrativo do Active Directory no computador onde o domínio B é o domínio local, você não consegue se conectar ao domínio A com o mesmo conjunto de credenciais na mesma instância do Centro Administrativo do Active Directory.

 Não há nenhuma associação de grupo mínima necessária para concluir este procedimento.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012: Para gerenciar um domínio externo na instância selecionada do Centro Administrativo do Active Directory usando o conjunto atual de credenciais de logon

1.  Para abrir o Centro Administrativo do Active Directory, em **Gerenciador do servidor**, clique em **ferramentas**e clique em **Centro Administrativo do Active Directory**.

    > [!NOTE]
    >  Outra maneira de abrir o Centro Administrativo do Active Directory é clicar **iniciar**e, em seguida, digite **dsac.exe**.

2.  Para abrir **adicionar nós de navegação**, clique em **gerenciar**, clique em **adicionar nós de navegação** conforme mostrado na ilustração a seguir.

     ![Captura de tela mostrando * * adicionar navegação nós * * UI](media/ADDS_ADACAddNavNode.gif)

3.  Em **adicionar nós de navegação**, clique em **conectar a outros domínios** conforme mostrado na ilustração a seguir.

     ![Captura de tela mostrando * * adicionar navegação nós * * UI](media/ADDS_ADACConnectToDomain.gif)

4.  Em **se conectar a**, digite o nome do domínio externo que você deseja gerenciar \ (por exemplo, **contoso.com**\) e, em seguida, clique em **Okey **.

5.  Quando você estiver conectado com êxito ao domínio externo, navegar através das colunas no **adicionar nós de navegação** janela, selecione um ou mais contêineres para adicionar ao painel de navegação Centro Administrativo do Active Directory e, em seguida, clique em **Okey**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2: Para gerenciar um domínio externo na instância selecionada do Centro Administrativo do Active Directory usando o conjunto atual de credenciais de logon

1.  Para abrir o Centro Administrativo do Active Directory, clique em **iniciar**, clique em **ferramentas administrativas**e clique em **Centro Administrativo do Active Directory**.

    > [!NOTE]
    >  Outra maneira de abrir o Centro Administrativo do Active Directory é clicar **iniciar**, clique em **executar**e, em seguida, digite **dsac.exe**.

2.  Para abrir **adicionar nós de navegação**, na parte superior da janela do Centro Administrativo do Active Directory, clique em **adicionar nós de navegação** conforme mostrado na ilustração a seguir.

     ![Captura de tela mostrando * * adicionar navegação nós * * UI](media/click_add_nav_nodes.gif)

    > [!NOTE]
    >  Outra maneira de abrir **adicionar nós de navegação** é right\ clique em qualquer lugar no espaço vazio no painel de navegação Centro Administrativo do Active Directory e, em seguida, clique em **adicionar nós de navegação**.

3.  Em **adicionar nós de navegação**, clique em **conectar a outros domínios** conforme mostrado na ilustração a seguir.

     ![Captura de tela mostrando * * adicionar navegação nós * * * * conectar-se a outros domínios * * da interface do Usuário](media/add_nav_nodes.gif)

4.  Em **se conectar a**, digite o nome do domínio externo que você deseja gerenciar \ (por exemplo, **contoso.com**\) e, em seguida, clique em **Okey **.

5.  Quando você estiver conectado com êxito ao domínio externo, navegar através das colunas no **adicionar nós de navegação** janela, selecione um ou mais contêineres para adicionar ao painel de navegação Centro Administrativo do Active Directory e, em seguida, clique em **Okey**.

 Para obter mais informações sobre como personalizar o painel de navegação do Centro Administrativo do Active Directory, consulte [personalizar Active Directory administrativas Centro de painel de navegação](customize-the-active-directory-administrative-center-navigation-pane.md).

 Você também pode abrir o Centro Administrativo do Active Directory usando um conjunto de credenciais de logon que é diferente de seu conjunto de credenciais de logon atual. O comando no procedimento a seguir pode ser útil se você estiver conectado ao computador que esteja executando o Centro Administrativo do Active Directory com credenciais de usuário normal, mas você deseja usar o Centro Administrativo do Active Directory neste computador para gerenciar seu domínio local como administrador. \ (Esse comando também pode ser útil se você quiser usar o Centro Administrativo do Active Directory para gerenciar remotamente um domínio externo que é diferente do seu domínio local com um conjunto de credenciais é diferente de seu conjunto de credenciais de logon atual. No entanto, o domínio estrangeiro deve ter uma relação de confiança estabelecida com o domínio local. \)

 Não há nenhuma associação de grupo mínima necessária para concluir este procedimento.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Para gerenciar um domínio usando credenciais de logon são diferentes do conjunto atual de credenciais de logon

1.  Para abrir o Centro Administrativo do Active Directory, em um prompt de comando, digite o seguinte comando e pressione ENTER:

     `runas /user:<domain\user> dsac`

     Onde `<domain\user>`é o conjunto de credenciais que você deseja abrir o Centro Administrativo do Active Directory com e `dsac`é o Centro Administrativo do Active Directory arquivo executável nome \(Dsac.exe\).

     Por exemplo, digite o seguinte comando e pressione ENTER:

     `runas /user:contoso\administrator dsac`

2.  Quando o Centro Administrativo do Active Directory é aberto, procure por meio do painel de navegação para exibir ou gerenciar seu domínio do Active Directory.

  

