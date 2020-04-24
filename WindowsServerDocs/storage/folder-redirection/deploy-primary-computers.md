---
title: Implantar computadores primários para Redirecionamento de Pastas e Perfis de Usuários Móveis
description: Como habilitar o suporte ao computador primário e designar computadores primários para usuários com Redirecionamento de Pastas e Perfis de Usuários Móveis.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: be2b41cf32e2020422c32415e2d8f4273eb09859
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "71394443"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Implantar computadores primários para Redirecionamento de Pastas e Perfis de Usuários Móveis

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

Este tópico descreve como habilitar o suporte a computador primário e designar computadores primários para usuários. Isso permite que você controle quais computadores usam o Redirecionamento de Pastas e Perfis de Usuários Móveis.

> [!IMPORTANT]
> Ao habilitar o suporte a computadores primários para Perfis de Usuários Móveis, sempre habilite o suporte do computador primário para o Redirecionamento de Pastas também. Isso mantém documentos e outros arquivos de usuário fora dos perfis de usuário, o que ajuda os perfis a permanecerem pequenos e os tempos de logon a permanecerem rápidos.

## <a name="prerequisites"></a>Pré-requisitos

## <a name="software-requirements"></a>Requisitos de software

O suporte a computadores primários tem os seguintes requisitos:

- O esquema AD DS (Active Directory Domain Services) deve ser atualizado para incluir adições de esquema do Windows Server 2012 (a instalação de um controlador de domínio do Windows Server 2012 atualizaria o esquema automaticamente). Para obter informações sobre como atualizar o esquema AD DS, confira [Integração do Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) e [Execução do Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Computadores cliente devem executar o Windows 10, o Windows 8.1, o Windows 8, o Windows Server 2019, o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012.

> [!TIP]
> Embora o suporte a computadores primários exija o Redirecionamento de Pastas e/ou Perfis de Usuários Móveis, se você está implantando essas tecnologias pela primeira vez, é melhor configurar o suporte do computador primário antes de habilitar os GPOs que configuram os Perfis de Usuários Móveis e o Redirecionamento de Pastas. Isso evita que os dados do usuário sejam copiados para computadores não primários antes de o suporte de computador primário ser habilitado. Para obter informações de configuração, confira [Implantar o Redirecionamento de Pastas](deploy-folder-redirection.md) e [Implantar Perfis de Usuários Móveis](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Etapa 1: Designar computadores primários para usuários

A primeira etapa na implantação do suporte a computadores primários é designar os computadores primários para cada usuário. Para fazer isso, use Centro de Administração do Active Directory para obter o nome diferenciado dos computadores relevantes e, em seguida, defina o atributo **msDs-PrimaryComputer**.

> [!TIP]
> Para usar o Windows PowerShell para trabalhar com computadores primários, confira a postagem no blog [Aprofundamento no Computador Primário do Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Veja como especificar os computadores primários para os usuários:

1. Abra o Gerenciador de Servidores em um computador com Ferramentas de Administração do Active Directory instaladas.
2. No menu **Ferramentas**, selecione **Centro de Administração do Active Directory**. O Centro de Administração do Active Directory é exibido.
3. Navegue até o contêiner **Computadores** no domínio apropriado.
4. Clique com o botão direito do mouse em um computador que você deseja designar como um computador primário e, em seguida, selecione **Propriedades**.
5. No painel de Navegação, selecione **Extensões**.
6. Selecione a guia **Editor de Atributo**, role para **distinguishedName**, selecione **Exibir**, clique com o botão direito do mouse no valor listado, selecione **Copiar**, selecione **OK** e, em seguida, selecione **Cancelar**.
7. Navegue para o contêiner **Usuários** no domínio apropriado, clique com o botão direito do mouse no usuário ao qual você deseja atribuir o computador, então selecione **Propriedades**.
8. No painel de Navegação, selecione **Extensões**.
9. Selecione a guia **Editor de Atributo**, selecione **msDs-PrimaryComputer** e, em seguida, selecione **Editar**. A caixa de diálogo Editor de Cadeia de Caracteres com Valores Múltiplos é exibida.
10. Clique com o botão direito do mouse na caixa de texto, selecione **Colar**, selecione **Adicionar**, selecione **OK** e, em seguida, selecione **OK** novamente.

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Etapa 2: Opcionalmente, habilite os computadores primários para o Redirecionamento de Pastas na Política de Grupo

A próxima etapa é configurar opcionalmente a Política de Grupo para habilitar o suporte a computadores primários para Redirecionamento de Pastas. Isso permite que as pastas de um usuário sejam redirecionadas em computadores designados como computadores primários do usuário, mas não em outros computadores. Você pode controlar os computadores primários para o Redirecionamento de Pastas por computador ou por usuário.

Veja como habilitar computadores primários para o Redirecionamento de Pastas:

1. No Gerenciamento de Política de Grupo, clique com o botão direito do mouse no GPO que você criou ao fazer a configuração inicial de Redirecionamento de Pastas e/ou Perfis de Usuários Móveis (por exemplo, **Configurações de Redirecionamento de Pastas** ou **Configurações de Perfis de Usuários Móveis**) e, em seguida, selecione **Editar**.
2. Para habilitar o suporte a computadores primários usando a Política de Grupo baseada em computador, navegue até **Configuração do Computador**. Para Política de Grupo baseada em usuário, navegue até **Configuração de Usuário**.
    - A Política de Grupo baseada em computador aplica o processamento de computador primário a todos os computadores aos quais o GPO se aplica, afetando todos os usuários dos computadores.
    - A Política de Grupo baseada em usuário aplica o processamento de computador primário a todas as contas de usuário às quais o GPO se aplica, afetando todos os computadores nos quais os usuários se conectam.
3. Em **Configuração do Computador** ou **Configuração do Usuário**, navegue até **Políticas**, **Modelos Administrativos**, **Sistema** e **Redirecionamento de Pastas**.
4. Clique com o botão direito do mouse em **Redirecionar pastas somente em computadores primários** e, em seguida, selecione **Editar**.
5. Selecione **Habilitado** e, em seguida, selecione **OK**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Etapa 3: Opcionalmente, habilite os computadores primários para Perfis de Usuários Móveis na Política de Grupo

A próxima etapa é configurar opcionalmente a Política de Grupo para habilitar o suporte a computadores primários para Perfis de Usuários Móveis. Isso permite que o perfil de um usuário use um perfil móvel em computadores designados como computadores primários do usuário, mas não em outros computadores.

Veja como habilitar computadores primários para Perfis de Usuários Móveis:

1. Habilite o suporte do computador primário para o Redirecionamento de Pastas, caso ainda não tenha feito isso.<br>Isso mantém documentos e outros arquivos de usuário fora dos perfis de usuário, o que ajuda os perfis a permanecerem pequenos e os tempos de logon a permanecerem rápidos.
2. Em Gerenciamento de Política de Grupo, clique com o botão direito do mouse no GPO que você criou (por exemplo, **Configurações de Redirecionamento de Pastas e Configurações de Perfis de Usuários Móveis**) e, em seguida, selecione **Editar**.
3. Navegue até **Configuração do Computador**, em seguida, **Políticas**, **Modelos Administrativos**, **Sistema**e **Perfis de Usuário**.
4. Clique com o botão direito do mouse em **Baixar perfis móveis somente em computadores primários** e, em seguida, selecione **Editar**.
5. Selecione **Habilitado** e, em seguida, selecione **OK**.

## <a name="step-4-enable-the-gpo"></a>Etapa 4: Habilitar o GPO

Depois de concluir a configuração de Redirecionamento de Pastas e Perfis de Usuários Móveis, habilite o GPO, se ainda não tiver feito isso. Isso permite que ele seja aplicado a usuários e computadores afetados.

Veja como habilitar o Redirecionamento de Pastas e/ou os GPOs de Perfis de Usuários Móveis:

1. Abra Gerenciamento de Política de Grupo
2. Clique com o botão direito do mouse nos GPOs criados por você e, em seguida, selecione **Vínculo Habilitado**. Uma caixa de seleção é exibida ao lado do item de menu.

## <a name="step-5-test-primary-computer-function"></a>Etapa 5: Testar a função do computador primário

Para testar o suporte do computador primário, entre em um computador primário, confirme se as pastas e os perfis são redirecionados, entre em um computador não primário e confirme se as pastas e os perfis não são redirecionados.

Faça o seguinte para testar a funcionalidade do computador primário:

1. Entre em um computador primário designado com uma conta de usuário para a qual você habilitou o Redirecionamento de Pastas e/ou os Perfis de Usuários Móveis.
2. Se a conta de usuário tiver se conectado anteriormente ao computador, abra uma sessão do Windows PowerShell ou uma janela de prompt de comando como administrador, digite o seguinte comando e desconecte quando solicitado para garantir que as configurações de Política de Grupo mais recentes sejam aplicadas ao computador cliente:

    ```PowerShell
    Gpupdate /force
    ```

3. Abra o Explorador de Arquivos.
1. Clique com o botão direito do mouse em uma pasta redirecionada (por exemplo, a pasta Meus Documentos na biblioteca de Documentos) e, em seguida, selecione **Propriedades**.
1. Selecione a guia **Localização** e confirme se o caminho exibe o compartilhamento de arquivo especificado, em vez de um caminho local. Para confirmar que o perfil do usuário é móvel, abra o **Painel de Controle**, selecione **Sistema e Segurança**, **Sistema**, **Configurações avançadas do sistema**, **Configurações** na seção Perfis do Usuário e procure por **Móvel** na coluna **Tipo**.
1. Entre com a mesma conta de usuário em um computador que não esteja designado como o computador primário do usuário.
1. Repita as etapas 2 a 5, procurando agora caminhos locais e um tipo de perfil **Local**.

> [!NOTE]
> Se as pastas tiverem sido redirecionadas em um computador antes da habilitação do suporte a computadores primários, as pastas permanecerão redirecionadas, a menos que a seguinte configuração esteja definida na configuração de política de Redirecionamento de Pastas de cada pasta: **Redirecione a pasta de volta para a localização do USERPROFILE local quando a política for removida**. Da mesma forma, os perfis que estavam anteriormente móveis em um determinado computador mostrarão **Roaming** nas colunas **Tipo**; no entanto, a coluna **Status** mostrará **Local**.

## <a name="more-information"></a>Mais informações

- [Implantar Redirecionamento de Pastas com Arquivos Offline](deploy-folder-redirection.md)
- [Implantar perfis de usuário móveis](deploy-roaming-user-profiles.md)
- [Visão geral de Redirecionamento de Pastas, Arquivos Offline e Perfis de Usuários Móveis](folder-redirection-rup-overview.md)
- [Aprofundando-se mais no Computador Primário do Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)