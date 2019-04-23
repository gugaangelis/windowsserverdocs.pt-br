---
title: Implantar computadores primários para redirecionamento de pasta e perfis de usuário móvel
description: Como habilitar o suporte de computador primário e designar computadores primários para usuários com o redirecionamento de pasta e perfis de usuário móvel.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7b3c87597e07102d00fc068b7ecd5744e4ba366f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854007"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Implantar computadores primários para redirecionamento de pasta e perfis de usuário móvel

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Este tópico descreve como habilitar o suporte de computador primário e designar computadores primários para usuários. Isso permite que você controle os computadores que usarão redirecionamento de pasta e perfis de usuário móvel.

>[!IMPORTANT]
>Ao habilitar o suporte de computador primário para perfis de usuário móvel, sempre habilite suporte a computadores primários para redirecionamento de pasta também. Isso impede que documentos e outros arquivos de usuário fora os perfis de usuário, que ajuda a perfis permanecem pequenos e tempos de logon rápido permanecer.

## <a name="prerequisites"></a>Pré-requisitos

## <a name="software-requirements"></a>Requisitos de software

Suporte de computador primário tem os seguintes requisitos:

- O esquema de serviços de domínio Active Directory (AD DS) deve ser atualizado para incluir adições de esquema do Windows Server 2012 (instalar um controlador de domínio do Windows Server 2012 automaticamente atualiza o esquema). Para obter informações sobre como atualizar o esquema do AD DS, consulte [integração Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) e [Adprep.exe executando](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

>[!TIP]
>Embora o suporte de computador primário exige o redirecionamento de pasta e/ou os perfis de usuário móvel, se você estiver implantando dessas tecnologias pela primeira vez, é melhor configurar o suporte de computador primário antes de habilitar os GPOs que configurar o redirecionamento de pasta e Perfis de usuário móvel. Isso evita que os dados do usuário sejam copiados para computadores não primários antes de o suporte de computador primário ser habilitado. Para obter informações de configuração, consulte [implantar o redirecionamento de pasta](deploy-folder-redirection.md) e [implantar perfis de usuário móvel](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Etapa 1: Designar computadores primários para usuários

A primeira etapa na implantação de suporte a computadores primários é designar os computadores primários para cada usuário. Para fazer isso, use o Centro de administração do Active Directory para obter o nome distinto de computadores relevantes e, em seguida, defina as **msDs-PrimaryComputer** atributo.

>[!TIP]
>Para usar o Windows PowerShell para trabalhar com computadores primários, consulte o postagem no blog [se aprofundar um pouco em computador primário do Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Aqui está como especificar os computadores primários para usuários:

1. Abra o Gerenciador de Servidores em um computador com Ferramentas de Administração do Active Directory instaladas.
2. Sobre o **ferramentas** menu, selecione **Centro de administração do Active Directory**. O Centro de Administração do Active Directory é exibido.
3. Navegue até a **computadores** contêiner no domínio apropriado.
4. Clique com botão direito a um computador que você deseja designar como um computador primário e, em seguida, selecione **propriedades**.
5. No painel de navegação, selecione **extensões**.
6. Selecione o **Editor de atributos** guia, role até **distinguishedName**, selecione **exibição**, o valor listado com o botão direito, selecione **cópia**, Selecione **Okey**e, em seguida, selecione **Cancelar**.
7. Navegue até a **os usuários** contêiner no domínio apropriado, clique com botão direito do usuário para o qual você deseja atribuir o computador e, em seguida, selecione **propriedades**.
8. No painel de navegação, selecione **extensões**.
9. Selecione o **Editor de atributos** guia, selecione **msDs-PrimaryComputer** e, em seguida, selecione **editar**. A caixa de diálogo Editor de Cadeia de Caracteres com Valores Múltiplos é exibida.
10. Clique com botão direito na caixa de texto, selecione **colar**, selecione **Add**, selecione **Okey**e, em seguida, selecione **Okey** novamente.

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Etapa 2: Opcionalmente, habilitar computadores primários para redirecionamento de pasta na política de grupo

A próxima etapa é configurar opcionalmente a política de grupo para habilitar o suporte a computadores primários para redirecionamento de pasta. Isso permite que as pastas de um usuário a ser redirecionada em computadores designados como computadores primários do usuário, mas não em outros computadores. Você pode controlar computadores primários para redirecionamento de pasta em uma base por computador ou por usuário.

Aqui está como habilitar os computadores primários para redirecionamento de pasta:

1. Em gerenciamento de diretiva de grupo, clique com botão direito no GPO criado por você ao fazer a configuração inicial do redirecionamento de pasta e/ou perfis de usuário móvel (por exemplo, **configurações de redirecionamento de pasta** ou **usuário de Roaming As configurações de perfis**) e, em seguida, selecione **editar**.
2. Para habilitar o suporte de computadores primários, usando a diretiva de grupo com base em computador, navegue até **configuração do computador**. Com base no usuário para política de grupo, navegue até **configuração do usuário**.
    - Política de grupo baseada em computador aplica-se o processamento de computador primário para todos os computadores aos quais o GPO se aplica, afetando todos os usuários dos computadores.
    - Com base no usuário a diretiva de grupo para aplica-se o processamento de computador primário para todas as contas de usuário ao qual o GPO se aplica, afetando todos os computadores aos quais os usuários de logon.
3. Sob **configuração do computador** ou **configuração do usuário**, navegue até **políticas**, em seguida, **modelos administrativos**, então  **Sistema**, em seguida, **redirecionamento de pasta**.
4. Clique com botão direito **redirecionar pastas somente nos computadores primários**e, em seguida, selecione **editar**.
5. Selecione **Enabled**e, em seguida, selecione **Okey**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Etapa 3: Opcionalmente, habilitar computadores primários para perfis de usuário móvel na diretiva de grupo

A próxima etapa é configurar opcionalmente a política de grupo para habilitar o suporte de computador primário para perfis de usuário móvel. Isso permite que um perfil de usuário façam roaming em computadores designados como computadores primários do usuário, mas não em outros computadores.

Aqui está como habilitar os computadores primários para perfis de usuário móvel:

1. Habilite suporte a computadores primários para redirecionamento de pasta, se você ainda não fez isso.
    * Isso impede que documentos e outros arquivos de usuário fora os perfis de usuário, que ajuda a perfis permanecem pequenos e tempos de logon rápido permanecer.
2. Em gerenciamento de diretiva de grupo, clique com botão direito no GPO criado por você (por exemplo, **redirecionamento de pastas e configurações de perfis de usuário móvel**) e, em seguida, selecione **editar**.
3. Navegue até **configuração do computador**, em seguida, **políticas**, em seguida, **modelos administrativos**, em seguida, **sistema**e, em seguida, **Perfis de usuário**.
4. Clique com botão direito **baixar perfis móveis somente, nos computadores primários** e, em seguida, selecione **editar**.
5. Selecione **Enabled**e, em seguida, selecione **Okey**.

## <a name="step-4-enable-the-gpo"></a>Etapa 4: Habilitar o GPO

Após configurar o redirecionamento de pasta e perfis de usuário móvel, habilitar o GPO se você ainda não tiver. Isso permite que ele seja aplicado a computadores e usuários afetados.

Aqui está como habilitar o redirecionamento de pasta e/ou os GPOs de perfis de usuário de Roaming:

1. Gerenciamento de política de grupo aberto
2. Os GPOs que você criou e, em seguida, selecione com o botão direito **vínculo habilitado**. Uma caixa de seleção deve aparecer ao lado do item de menu.

## <a name="step-5-test-primary-computer-function"></a>Etapa 5: Testar função de computador primário

Para testar o suporte de computador primário, entre em um computador primário, confirme que as pastas e os perfis são redirecionados, em seguida, entre um computador não primário e confirme que as pastas e os perfis não são redirecionados.

Aqui está como testar a funcionalidade de computador primário:

1. Entre em um computador primário designado com uma conta de usuário para o qual você habilitou o redirecionamento de pasta e/ou perfis de usuário móvel.
2. Se a conta de usuário tiver se conectado anteriormente no computador, abra uma janela de Prompt de comando como administrador ou sessão do Windows PowerShell, digite o seguinte comando e saia em seguida, quando for solicitado para garantir que as configurações de diretiva de grupo mais recentes sejam aplicadas para o computador cliente:
    ```PowerShell
    Gpupdate /force
    ```
3. Abra o Explorador de Arquivos.
4. Clique em uma pasta redirecionada (por exemplo, a pasta Meus documentos da biblioteca de documentos) e, em seguida, selecione **propriedades**.
5. Selecione o **local** guia e confirme se o caminho de compartilhamento de arquivos especificado, em vez de um caminho local é exibida. Para confirmar que o perfil do usuário está em roaming, abra **painel de controle**, selecione **sistema e segurança**, selecione **sistema**, selecione **configurações avançadas do sistema** , selecione **configurações** nos perfis de usuário seção e, em seguida, procure **Roaming** no **tipo** coluna.
6. Entrar com a mesma conta de usuário para um computador que não esteja designado como computador primário do usuário.
7. Repita as etapas 2 a 5, em vez disso, procurando os caminhos de locais e um **Local** tipo de perfil.

>[!NOTE]
>Se a pastas foram redirecionadas em um computador antes de habilitar o suporte de computador primário, as pastas permanecerão redirecionadas, a menos que a seguinte configuração é definida na configuração de diretiva de redirecionamento de pasta de cada pasta: **Redirecionar a pasta de volta para o perfil de usuário local quando a política for removida**. Da mesma forma, mostrará os perfis que foram anteriormente roaming em um computador específico **Roaming** na **tipo** colunas; no entanto, o **Status** coluna mostrará **Local**.

## <a name="more-information"></a>Mais informações

- [Implantar o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md)
- [Implantar perfis de usuário móvel](deploy-roaming-user-profiles.md)
- [Visão geral do redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](folder-redirection-rup-overview.md)
- [Se aprofundar um pouco no computador primário do Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)