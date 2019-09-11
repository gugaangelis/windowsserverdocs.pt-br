---
title: Implantar computadores primários para redirecionamento de pasta e perfis de usuário de roaming
description: Como habilitar o suporte ao computador primário e designar computadores primários para usuários com redirecionamento de pasta e perfis de usuário de roaming.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: fe026b97f15b4094303c8162c5363cc6205dedd1
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70867281"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Implantar computadores primários para redirecionamento de pasta e perfis de usuário de roaming

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Este tópico descreve como habilitar o suporte a computador primário e designar computadores primários para usuários. Isso permite que você controle quais computadores usam o redirecionamento de pasta e perfis de usuário de roaming.

> [!IMPORTANT]
> Ao habilitar o suporte a computadores primários para perfis de usuário de roaming, sempre habilite o suporte do computador primário para o redirecionamento de pasta também. Isso mantém documentos e outros arquivos de usuário fora dos perfis de usuário, o que ajuda os perfis a permanecerem pequenos e os tempos de logon permanecem rápidos.

## <a name="prerequisites"></a>Pré-requisitos

## <a name="software-requirements"></a>Requisitos de software

O suporte a computadores primários tem os seguintes requisitos:

- O esquema de Active Directory Domain Services (AD DS) deve ser atualizado para incluir adições de esquema do Windows Server 2012 (a instalação de um controlador de domínio do Windows Server 2012 atualiza automaticamente o esquema). Para obter informações sobre como atualizar o esquema de AD DS, consulte [integração adprep. exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) e [executando adprep. exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Os computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

> [!TIP]
> Embora o suporte a computadores primários exija o redirecionamento de pasta e/ou perfis de usuário de roaming, se você estiver implantando essas tecnologias pela primeira vez, é melhor configurar o suporte do computador primário antes de habilitar os GPOs que configuram o redirecionamento de pasta e Perfis de usuário de roaming. Isso evita que os dados do usuário sejam copiados para computadores não primários antes de o suporte de computador primário ser habilitado. Para obter informações de configuração, consulte [implantar o redirecionamento de pasta](deploy-folder-redirection.md) e [implantar perfis de usuário de roaming](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Etapa 1: Designar computadores primários para usuários

A primeira etapa na implantação do suporte a computadores primários é designar os computadores primários para cada usuário. Para fazer isso, use Active Directory centro de administração para obter o nome distinto dos computadores relevantes e, em seguida, defina o atributo **msDS-PrimaryComputer** .

> [!TIP]
> Para usar o Windows PowerShell para trabalhar com computadores primários, consulte a postagem do blog que está se [aprofundando no computador primário do Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Veja como especificar os computadores primários para os usuários:

1. Abra o Gerenciador de Servidores em um computador com Ferramentas de Administração do Active Directory instaladas.
2. No menu **ferramentas** , selecione **Active Directory centro de administração**. O Centro de Administração do Active Directory é exibido.
3. Navegue até o contêiner **computadores** no domínio apropriado.
4. Clique com o botão direito do mouse em um computador que você deseja designar como um computador primário e selecione **Propriedades**.
5. No painel de navegação, selecione **extensões**.
6. Selecione a **guia Editor de atributo** , role até **distinguishedName**, selecione **Exibir**, clique com o botão direito do mouse no valor listado, selecione **copiar**, selecione **OK**e, em seguida, selecione **Cancelar**.
7. Navegue até o contêiner **usuários** no domínio apropriado, clique com o botão direito do mouse no usuário ao qual você deseja atribuir o computador e selecione **Propriedades**.
8. No painel de navegação, selecione **extensões**.
9. Selecione a guia **Editor de atributo** , selecione **msDS-PrimaryComputer** e, em seguida, selecione **Editar**. A caixa de diálogo Editor de Cadeia de Caracteres com Valores Múltiplos é exibida.
10. Clique com o botão direito do mouse na caixa de texto, selecione **colar**, **Adicionar**, selecionar **OK**e, em seguida, selecione **OK** novamente.

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Etapa 2: Opcionalmente, habilite os computadores primários para o redirecionamento de pasta no Política de Grupo

A próxima etapa é configurar opcionalmente Política de Grupo para habilitar o suporte a computadores primários para o redirecionamento de pasta. Isso permite que as pastas de um usuário sejam redirecionadas em computadores designados como computadores primários do usuário, mas não em outros computadores. Você pode controlar os computadores primários para o redirecionamento de pasta por computador ou uma base por usuário.

Veja como habilitar computadores primários para o redirecionamento de pasta:

1. No gerenciamento de Política de Grupo, clique com o botão direito do mouse no GPO que você criou ao fazer a configuração inicial de redirecionamento de pasta e/ou perfis de usuário de roaming (por exemplo, **configurações de redirecionamento de pasta** ou **configurações de perfis de usuário móvel**) e, em seguida, Selecione **Editar**.
2. Para habilitar o suporte a computadores primários usando Política de Grupo baseadas em computador, navegue até **configuração do computador**. Para Política de Grupo baseadas no usuário, navegue até **configuração do usuário**.
    - O Política de Grupo baseado em computador aplica o processamento de computador primário a todos os computadores aos quais o GPO se aplica, afetando todos os usuários dos computadores.
    - O Política de Grupo baseado no usuário aplica o processamento de computador primário a todas as contas de usuário às quais o GPO se aplica, afetando todos os computadores nos quais os usuários se conectam.
3. Em **configuração do computador** ou **configuração do usuário**, navegue até **políticas**, depois **modelos administrativos**, **sistema**e **redirecionamento de pasta**.
4. Clique com o botão direito do mouse em **redirecionar pastas somente em computadores primários**e selecione **Editar**.
5. Selecione **habilitado**e, em seguida, selecione **OK**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Etapa 3: Opcionalmente, habilite os computadores primários para perfis de usuário móvel no Política de Grupo

A próxima etapa é configurar opcionalmente Política de Grupo para habilitar o suporte a computadores primários para perfis de usuário de roaming. Isso permite que o perfil de um usuário faça roaming em computadores designados como computadores primários do usuário, mas não em outros computadores.

Veja como habilitar computadores primários para perfis de usuário de roaming:

1. Habilite o suporte do computador primário para o redirecionamento de pasta, caso ainda não tenha feito isso.<br>Isso mantém documentos e outros arquivos de usuário fora dos perfis de usuário, o que ajuda os perfis a permanecerem pequenos e os tempos de logon permanecem rápidos.
2. Em gerenciamento de Política de Grupo, clique com o botão direito do mouse no GPO que você criou (por exemplo, **redirecionamento de pasta e configurações de perfis de usuário de roaming**) e, em seguida, selecione **Editar**.
3. Navegue até **configuração do computador**, em seguida, **políticas**, depois **modelos administrativos**, **sistema**e **perfis de usuário**.
4. Clique com o botão direito do mouse em **baixar perfis móveis somente em computadores primários** e selecione **Editar**.
5. Selecione **habilitado**e, em seguida, selecione **OK**.

## <a name="step-4-enable-the-gpo"></a>Etapa 4: Habilitar o GPO

Depois de concluir a configuração de redirecionamento de pasta e perfis de usuário de roaming, habilite o GPO, se ainda não tiver feito isso. Isso permite que ele seja aplicado a usuários e computadores afetados.

Veja como habilitar o redirecionamento de pasta e/ou os GPOs de perfis de usuário de roaming:

1. Abrir gerenciamento de Política de Grupo
2. Clique com o botão direito do mouse nos GPOs que você criou e selecione **link habilitado**. Uma caixa de seleção deve aparecer ao lado do item de menu.

## <a name="step-5-test-primary-computer-function"></a>Etapa 5: Função do computador primário de teste

Para testar o suporte do computador primário, entre em um computador primário, confirme se as pastas e os perfis são redirecionados, entre em um computador não primário e confirme se as pastas e os perfis não são redirecionados.

Veja como testar a funcionalidade do computador primário:

1. Entre em um computador primário designado com uma conta de usuário para a qual você habilitou o redirecionamento de pasta e/ou perfis de usuário de roaming.
2. Se a conta de usuário tiver se conectado anteriormente ao computador, abra uma sessão do Windows PowerShell ou uma janela de prompt de comando como administrador, digite o comando a seguir e, em seguida, desconecte quando solicitado para garantir que as configurações de Política de Grupo mais recentes sejam aplicadas ao computador cliente:

    ```PowerShell
    Gpupdate /force
    ```

3. Abra o Explorador de Arquivos.
1. Clique com o botão direito do mouse em uma pasta redirecionada (por exemplo, a pasta meus documentos na biblioteca de documentos) e selecione **Propriedades**.
1. Selecione a guia **local** e confirme se o caminho exibe o compartilhamento de arquivos especificado, em vez de um caminho local. Para confirmar se o perfil do usuário está em roaming, abra o **painel de controle**, selecione **sistema e segurança**, selecione **sistema**, selecione **Configurações avançadas do sistema**, selecione **configurações** na seção perfis de usuário e procure  **Roaming** na coluna **Type** .
1. Entre com a mesma conta de usuário em um computador que não esteja designado como o computador primário do usuário.
1. Repita as etapas 2 a 5, procurando caminhos locais e um tipo de perfil **local** .

> [!NOTE]
> Se as pastas foram redirecionadas em um computador antes da habilitação do suporte a computadores primários, as pastas permanecerão redirecionadas, a menos que a configuração a seguir esteja definida na configuração de política de redirecionamento de pasta de cada pasta: **Redirecione a pasta de volta para o local do USERPROFILE local quando a política for removida**. Da mesma forma, os perfis que estavam anteriormente em roaming em um determinado computador mostrarão **roaming** nas colunas de **tipo** ; no entanto, a coluna **status** mostrará **local**.

## <a name="more-information"></a>Mais informações

- [Implantar redirecionamento de pasta com Arquivos Offline](deploy-folder-redirection.md)
- [Implantar perfis de usuário móveis](deploy-roaming-user-profiles.md)
- [Visão geral dos perfis de usuário de redirecionamento de pasta, Arquivos Offline e roaming](folder-redirection-rup-overview.md)
- [Aprofundando-se mais no computador primário do Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)