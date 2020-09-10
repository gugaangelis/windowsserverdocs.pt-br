---
title: Gerenciar domínios diferentes no Centro Administrativo do Active Directory
description: Segurança do Windows Server
ms.assetid: 166351c3-4076-48be-aa8f-797adf1e9d68
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/12/2016
ms.openlocfilehash: c793c30c6637a050d2b2cbb055ec289e3c3e8f20
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89638034"
---
# <a name="manage-different-domains-in-active-directory-administrative-center"></a>Gerenciar domínios diferentes no Centro Administrativo do Active Directory

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

  Quando você abre Active Directory administrativas, o domínio no qual você está conectado no momento neste computador \( o domínio local \) aparece no painel de navegação centro administrativo do Active Directory no painel \( esquerdo \) . Dependendo dos direitos do seu conjunto atual de credenciais de logon, você poderá exibir ou gerenciar os objetos do Active Directory no domínio local.

 Você também pode usar o mesmo conjunto de credenciais de logon e a mesma instância do Centro Administrativo do Active Directory para exibir ou gerenciar Active Directory objetos em qualquer outro domínio na mesma floresta ou um domínio em outra floresta que tenha uma relação de confiança estabelecida com o domínio local. Há suporte para relações de confiança unidirecionais \- e duas \- formas de confiança.

> [!NOTE]
>  Se houver uma \- confiança unidirecional entre o domínio a e o domínio B por meio do qual os usuários no domínio a podem acessar recursos no domínio b, mas os usuários no domínio b não poderão acessar recursos no domínio a, se você estiver executando centro administrativo do Active Directory no computador em que o domínio a é o domínio local, poderá se conectar ao domínio B com o conjunto atual de credenciais de logon e na mesma centro administrativo do Active Directory instância No entanto, se você estiver executando o Centro Administrativo no computador em que B é o domínio local, não será possível se conectar ao Domínio A com o mesmo conjunto de credenciais, na mesma instância do Centro Administrativo.

 Nenhum requisito mínimo de associação a grupo é necessário para a conclusão deste procedimento.

### <a name="windows-server-2012-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2012: para gerenciar um domínio externo na instância selecionada do Centro Administrativo do Active Directory usando o conjunto atual de credenciais de logon

1.  Para abrir o Centro Administrativo do Active Directory, em **Gerenciador do servidor**, clique em **ferramentas**e, em seguida, clique em **centro administrativo do Active Directory**.

    > [!NOTE]
    >  Outra maneira de abrir o Centro Administrativo do Active Directory é clicar em **Iniciar**e digitar **dsac.exe**.

2.  Para abrir **adicionar nós de navegação**, clique em **gerenciar**e, em seguida, clique em **adicionar nós de navegação** , conforme mostrado na ilustração a seguir.

     ![Captura de tela mostrando * * adicionar nós de navegação * * interface do usuário](media/ADDS_ADACAddNavNode.gif)

3.  Em **adicionar nós de navegação**, clique em **conectar a outros domínios** , conforme mostrado na ilustração a seguir.

     ![Captura de tela mostrando * * adicionar nós de navegação * * interface do usuário](media/ADDS_ADACConnectToDomain.gif)

4.  Em **conectar a**, digite o nome do domínio externo que você deseja gerenciar, \( por exemplo, **contoso.com** \) , e clique em **OK**.

5.  Quando tiver conectado com êxito ao domínio externo, navegue pelas colunas da janela **Adicionar Nós de Navegação**, selecione o contêiner ou contêineres a serem adicionados ao painel de navegação do Centro Administrativo do Active Directory e clique em **OK**.

### <a name="windows-server-2008-r2-to-manage-a-foreign-domain-in-the-selected-instance-of-active-directory-administrative-center-using-the-current-set-of-logon-credentials"></a>Windows Server 2008 R2: para gerenciar um domínio externo na instância selecionada do Centro Administrativo do Active Directory usando o conjunto de credenciais de logon atual

1. Para abrir a Central Administrativa do Active Directory, clique em **Iniciar**, clique em **Ferramentas Administrativas** e em **Central Administrativa do Active Directory**.

   > [!NOTE]
   >  Outra maneira de abrir o Centro Administrativo do Active Directory é clicar em **Iniciar**, **executar**e digitar **dsac.exe**.

2. Para abrir **adicionar nós de navegação**, próximo à parte superior da janela de centro administrativo do Active Directory, clique em **adicionar nós de navegação** , conforme mostrado na ilustração a seguir.

    ![Captura de tela mostrando * * adicionar nós de navegação * * interface do usuário](media/click_add_nav_nodes.gif)

   > [!NOTE]
   >  Outra maneira de abrir **adicionar nós de navegação** é clicar com o botão direito do mouse em \- qualquer lugar no espaço vazio no painel de navegação centro administrativo do Active Directory e clicar em **adicionar nós de navegação**.

3. Em **adicionar nós de navegação**, clique em **conectar a outros domínios** , conforme mostrado na ilustração a seguir.

    ![Captura de tela mostrando * * adicionar nós de navegação * * * * conectar a outros domínios * * interface do usuário](media/add_nav_nodes.gif)

4. Em **conectar a**, digite o nome do domínio externo que você deseja gerenciar, \( por exemplo, **contoso.com** \) , e clique em **OK**.

5. Quando tiver conectado com êxito ao domínio externo, navegue pelas colunas da janela **Adicionar Nós de Navegação**, selecione o contêiner ou contêineres a serem adicionados ao painel de navegação do Centro Administrativo do Active Directory e clique em **OK**.

   Para obter mais informações sobre como personalizar o painel de navegação Centro Administrativo do Active Directory, consulte [Personalizar o painel de navegação centro administrativo do Active Directory](customize-the-active-directory-administrative-center-navigation-pane.md).

   Você também pode abrir o Centro Administrativo usando um conjunto de credenciais de logon que seja diferente do conjunto atual. O comando no procedimento a seguir poderá ser útil caso você tenha efetuado logon com credenciais de usuário normal, mas deseje usar o Centro Administrativo para gerenciar o domínio local como administrador. \(Esse comando também pode ser útil se você quiser usar Centro Administrativo do Active Directory para gerenciar remotamente um domínio externo diferente do seu domínio local com um conjunto de credenciais que seja diferente do seu conjunto atual de credenciais de logon. No entanto, o domínio externo deve ter uma relação de confiança estabelecida com o domínio local.\)

   Nenhum requisito mínimo de associação a grupo é necessário para a conclusão deste procedimento.

### <a name="to-manage-a-domain-using-logon-credentials-that-are-different-from-the-current-set-of-logon-credentials"></a>Para gerenciar um domínio usando credenciais de logon diferentes do conjunto de credenciais atuais

1.  Para abrir o Centro Administrativo, no prompt de comando, digite o comando a seguir e pressione ENTER:

     `runas /user:<domain\user> dsac`

     Em que `<domain\user>` é o conjunto de credenciais que você deseja abrir centro administrativo do Active Directory com e `dsac` é o centro administrativo do Active Directory nome do arquivo executável \(Dsac.exe\) .

     Por exemplo, digite o comando a seguir e pressione ENTER:

     `runas /user:contoso\administrator dsac`

2.  Quando o Centro Administrativo estiver aberto, navegue pelos painéis de navegação para exibir ou gerenciar o domínio Active Directory.



