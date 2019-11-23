---
title: Gerenciar domínios diferentes no Centro Administrativo do Active Directory
ms.prod: windows-server
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
ms.openlocfilehash: 71edf6bb38cc665fe5c780ce986d0c0b8807d6ab
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386932"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Gerenciar domínios diferentes no Centro Administrativo do Active Directory

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

  Quando você abre Active Directory administrativas, o domínio no qual você está conectado no momento neste computador \(o\) de domínio local aparece no painel de navegação Centro Administrativo do Active Directory \(painel esquerdo\). Dependendo dos direitos do seu conjunto atual de credenciais de logon, você pode exibir ou gerenciar os objetos de Active Directory nesse domínio local.

 Você também pode usar o mesmo conjunto de credenciais de logon e a mesma instância do Centro Administrativo do Active Directory para exibir ou gerenciar Active Directory objetos em qualquer outro domínio na mesma floresta ou um domínio em outra floresta que tenha uma relação de confiança estabelecida com o local controlador. Um\-maneira de confiar e duas\-como as relações de confiança têm suporte.

> [!NOTE]
>  Se houver uma relação de confiança\-entre o domínio A e o domínio B por meio do qual os usuários no domínio A podem acessar recursos no domínio B, mas os usuários no domínio B não poderão acessar recursos no domínio A, se você estiver executando Centro Administrativo do Active Directory no computador em que o domínio A é o domínio local, poderá se conectar ao domínio B com o conjunto atual de credenciais de logon e na mesma Centro Administrativo do Active Directory instância Mas se você estiver executando Centro Administrativo do Active Directory no computador em que o domínio B é seu domínio local, não poderá se conectar ao domínio A com o mesmo conjunto de credenciais na mesma instância do Centro Administrativo do Active Directory.

 Nenhum requisito mínimo de associação a grupo é necessário para a conclusão deste procedimento.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012: para gerenciar um domínio externo na instância selecionada do Centro Administrativo do Active Directory usando o conjunto atual de credenciais de logon

1.  Para abrir o Centro Administrativo do Active Directory, em **Gerenciador do servidor**, clique em **ferramentas**e, em seguida, clique em **centro administrativo do Active Directory**.

    > [!NOTE]
    >  Outra maneira de abrir o Centro Administrativo do Active Directory é clicar em **Iniciar**e, em seguida, digitar **dsac. exe**.

2.  Para abrir **adicionar nós de navegação**, clique em **gerenciar**e, em seguida, clique em **adicionar nós de navegação** , conforme mostrado na ilustração a seguir.

     ![Captura de tela mostrando * * adicionar nós de navegação * * interface do usuário](media/ADDS_ADACAddNavNode.gif)

3.  Em **adicionar nós de navegação**, clique em **conectar a outros domínios** , conforme mostrado na ilustração a seguir.

     ![Captura de tela mostrando * * adicionar nós de navegação * * interface do usuário](media/ADDS_ADACConnectToDomain.gif)

4.  Em **conectar a**, digite o nome do domínio externo que você deseja gerenciar \(por exemplo, **contoso.com**\)e clique em **OK**.

5.  Quando você estiver conectado com êxito ao domínio externo, navegue pelas colunas na janela **adicionar nós de navegação** , selecione o contêiner ou contêineres a serem adicionados ao seu centro administrativo do Active Directory painel de navegação e clique em **OK**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2: para gerenciar um domínio externo na instância selecionada do Centro Administrativo do Active Directory usando o conjunto de credenciais de logon atual

1. Para abrir Centro Administrativo do Active Directory, clique em **Iniciar**, **Ferramentas administrativas**e, em seguida, clique em **centro administrativo do Active Directory**.

   > [!NOTE]
   >  Outra maneira de abrir o Centro Administrativo do Active Directory é clicar em **Iniciar**, **executar**e, em seguida, digitar **dsac. exe**.

2. Para abrir **adicionar nós de navegação**, próximo à parte superior da janela de centro administrativo do Active Directory, clique em **adicionar nós de navegação** , conforme mostrado na ilustração a seguir.

    ![Captura de tela mostrando * * adicionar nós de navegação * * interface do usuário](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  Outra maneira de abrir **adicionar nós de navegação** é o direito\-clicar em qualquer lugar no espaço vazio no painel de navegação centro administrativo do Active Directory e, em seguida, clicar em **adicionar nós de navegação**.

3. Em **adicionar nós de navegação**, clique em **conectar a outros domínios** , conforme mostrado na ilustração a seguir.

    ![Captura de tela mostrando * * adicionar nós de navegação * * * * conectar a outros domínios * * interface do usuário](media/add_nav_nodes.gif)

4. Em **conectar a**, digite o nome do domínio externo que você deseja gerenciar \(por exemplo, **contoso.com**\)e clique em **OK**.

5. Quando você estiver conectado com êxito ao domínio externo, navegue pelas colunas na janela **adicionar nós de navegação** , selecione o contêiner ou contêineres a serem adicionados ao seu centro administrativo do Active Directory painel de navegação e clique em **OK**.

   Para obter mais informações sobre como personalizar o painel de navegação Centro Administrativo do Active Directory, consulte [Personalizar o painel de navegação centro administrativo do Active Directory](customize-the-active-directory-administrative-center-navigation-pane.md).

   Você também pode abrir Centro Administrativo do Active Directory usando um conjunto de credenciais de logon diferente do seu conjunto atual de credenciais de logon. O comando no procedimento a seguir pode ser útil se você estiver conectado ao computador que está executando Centro Administrativo do Active Directory com as credenciais de usuário normais, mas quiser usar Centro Administrativo do Active Directory neste computador para gerenciar seu domínio local como administrador. \(esse comando também poderá ser útil se você quiser usar Centro Administrativo do Active Directory para gerenciar remotamente um domínio externo diferente do seu domínio local com um conjunto de credenciais que seja diferente do seu conjunto atual de credenciais de logon. No entanto, o domínio externo deve ter uma relação de confiança estabelecida com o domínio local.\)

   Nenhum requisito mínimo de associação a grupo é necessário para a conclusão deste procedimento.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Para gerenciar um domínio usando credenciais de logon diferentes do conjunto de credenciais de logon atual

1.  Para abrir Centro Administrativo do Active Directory, em um prompt de comando, digite o seguinte comando e pressione ENTER:

     `runas /user:<domain\user> dsac`

     Em que `<domain\user>` é o conjunto de credenciais que você deseja abrir Centro Administrativo do Active Directory e `dsac` é o Centro Administrativo do Active Directory nome do arquivo executável \(dsac. exe\).

     Por exemplo, digite o seguinte comando e pressione ENTER:

     `runas /user:contoso\administrator dsac`

2.  Quando Centro Administrativo do Active Directory estiver aberto, navegue pelo painel de navegação para exibir ou gerenciar seu domínio de Active Directory.

  

