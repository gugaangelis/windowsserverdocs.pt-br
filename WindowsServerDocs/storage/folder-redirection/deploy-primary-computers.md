---
title: Implantar computadores principais para o redirecionamento de pasta e perfis de usuário
description: Como habilitar o suporte de computador principal e designar principais computadores para usuários com redirecionamento de pasta e perfis de usuário móvel.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 09/10/2018
ms.localizationpriority: medium
ms.openlocfilehash: 7b3c87597e07102d00fc068b7ecd5744e4ba366f
ms.sourcegitcommit: 505505b7ec99506e7ff50eddbdd6aa94a602abe6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/11/2018
ms.locfileid: "3831396"
---
# Implantar computadores principais para o redirecionamento de pasta e perfis de usuário

>Aplica-se a: Windows 10, Windows 8, Windows 8.1, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016

Este tópico descreve como habilitar o suporte de computador principal e designar os computadores principais para usuários. Isso permite que você controle quais computadores usar redirecionamento de pasta e perfis de usuário.

>[!IMPORTANT]
>Ao habilitar o suporte de computador primário para perfis de usuário móvel, sempre ative o suporte de computador principal também redirecionamento de pasta. Isso mantém documentos e outros arquivos de usuário fora os perfis de usuário, que ajuda a perfis permanecem pequeno e logon vezes permanecer rápido.

## Pré-requisitos

## Requisitos de software

Suporte de computador primário tem os seguintes requisitos:

- O esquema do Active Directory Domain Services (AD DS) deve ser atualizado para incluir adições de esquema do Windows Server 2012 (instalar um controlador de domínio do Windows Server 2012 automaticamente atualiza o esquema). Para obter informações sobre como atualizar o esquema do AD DS, consulte [integração Adprep.exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) e [Adprep.exe em execução](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- Os computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012.

>[!TIP]
>Embora o suporte de computador principal requer o redirecionamento de pasta e/ou perfis de usuário móvel, se você estiver implantando essas tecnologias pela primeira vez, é melhor configurar o suporte de computador principal antes de habilitar os GPOs que configurar o redirecionamento de pasta e Perfis de usuário móvel. Isso impede que os dados do usuário sejam copiadas para computadores não principal antes de suporte de computador principal está habilitado. Para obter informações de configuração, consulte [Implantar redirecionamento de pasta](deploy-folder-redirection.md) e [Implantar perfis de usuário móvel](deploy-roaming-user-profiles.md).

## Etapa 1: Designar computadores principais para usuários

A primeira etapa na implantação de suporte de computadores principal é designar os computadores principais para cada usuário. Para fazer isso, use o Centro de administração do Active Directory para obter o nome distinto dos computadores relevantes e, em seguida, defina o atributo **msDs-PrimaryComputer** .

>[!TIP]
>Para usar o Windows PowerShell para trabalhar com computadores principais, consulte o blog lançar [uma análise mais profunda um pouco no computador principal do Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Aqui está como especificar os computadores principais para os usuários:

1. Abra o Gerenciador do Servidor em um computador com Ferramentas de Administração do Active Directory instaladas.
2. No menu **Ferramentas** , selecione o **Centro de administração do Active Directory**. O Centro de Administração do Active Directory é exibido.
3. Navegue até o contêiner de **computadores** no domínio apropriado.
4. Clique com botão direito a um computador que você deseja designar como um computador principal e, em seguida, selecione **Propriedades**.
5. No painel de navegação, selecione **extensões**.
6. Selecione a guia do **Editor de atributos** , role até **distinguishedName**, selecione o **modo de exibição**, clique com botão direito o valor listado, selecione **cópia**, selecione **Okey**e, em seguida, selecione **Cancelar**.
7. Navegue até o contêiner **usuários** no domínio apropriado, clique com botão direito do usuário ao qual você deseja atribuir o computador e, em seguida, selecione **Propriedades**.
8. No painel de navegação, selecione **extensões**.
9. Selecione a guia do **Editor de atributos** , selecione **msDs-PrimaryComputer** e, em seguida, selecione **Editar**. A caixa de diálogo Editor de Cadeia de Caracteres com Valores Múltiplos é exibida.
10. Clique com botão direito na caixa de texto, selecione **Colar**, selecione **Add**, selecione **Okey**e, em seguida, selecione **Okey** novamente.

## Etapa 2: Habilitar opcionalmente computadores principais para o redirecionamento de pasta na política de grupo

A próxima etapa é configurar opcionalmente a política de grupo para habilitar o suporte de computador principal para o redirecionamento de pasta. Isso permite que as pastas de um usuário seja redirecionado em computadores designados como computadores principal do usuário, mas não em outros computadores. Você pode controlar computadores principais para redirecionamento de pasta em uma base por computador ou por usuário.

Veja como habilitar computadores principais para o redirecionamento de pasta:

1. No gerenciamento de política de grupo, clique com botão direito no GPO que você criou ao fazer a configuração inicial de redirecionamento de pasta e/ou perfis de usuário móvel (por exemplo, **As configurações de redirecionamento de pasta** ou **Configurações de perfis de usuário móvel**) e, em seguida, Selecione **Editar**.
2. Para habilitar o suporte a computadores principal usando política de grupo com base em computador, navegue até **Configuração do computador**. Com base em usuário diretiva de grupo, navegue até **Configuração do usuário**.
    - Política de grupo com base em computador aplica processamento de computador principal para todos os computadores aos quais o GPO aplica, que afetam todos os usuários dos computadores.
    - Com base em usuário a política de grupo para se aplica processamento de computador principal para todas as contas de usuário ao qual o GPO aplica, que afetam todos os computadores aos quais os usuários logon.
3. Em **Configuração do computador** ou **Configuração do usuário**, navegue até **políticas**, em seguida, **Modelos administrativos**, em seguida, **sistema**e **Redirecionamento de pasta**.
4. Clique com botão direito **redirecionar pastas principais apenas em computadores**e, em seguida, selecione **Editar**.
5. Selecionar **ativado**e, em seguida, selecione **Okey**.

## Etapa 3: Habilitar opcionalmente computadores principais para perfis de usuário móvel na política de grupo

A próxima etapa é configurar opcionalmente a política de grupo para habilitar o suporte de computador primário para perfis de usuário móvel. Isso permite que um perfil de usuário mobilidade em computadores designados como computadores principal do usuário, mas não em outros computadores.

Veja como habilitar computadores principais para perfis de usuário:

1. Habilite o suporte de computador principal para redirecionamento de pasta, se você ainda não estiver.
    * Isso mantém documentos e outros arquivos de usuário fora os perfis de usuário, que ajuda a perfis permanecem pequeno e logon vezes permanecer rápido.
2. No gerenciamento de política de grupo, clique com botão direito no GPO que você criou (por exemplo, **redirecionamento de pasta e configurações de perfis de usuário móvel**) e, em seguida, selecione **Editar**.
3. Navegue até **configuração do computador**, e **políticas**, em seguida, **modelos administrativos**, e **sistema**e, em seguida, **perfis de usuário**.
4. Clique com botão direito **Baixar perfis móveis principais apenas em computadores** e, em seguida, selecione **Editar**.
5. Selecionar **ativado**e, em seguida, selecione **Okey**.

## Etapa 4: Habilitar o GPO

Após configurar o redirecionamento de pasta e perfis de usuário, habilite o GPO se você ainda não tiver feito. Isso permite que ele sejam aplicadas a usuários afetados e computadores.

Veja como habilitar o redirecionamento de pasta e/ou GPOs de perfis de usuário móvel:

1. Gerenciamento de política de grupo aberto
2. Clique com botão direito os GPOs que você criou e, em seguida, selecione o **Link habilitado**. Uma caixa de seleção deve aparecer ao lado do item de menu.

## Etapa 5: Testar a função principal do computador

Para testar o suporte de computador primário, entrar em um computador principal, confirme que as pastas e os perfis são redirecionados, em seguida, entrar em um computador não principal e confirme que as pastas e os perfis não são redirecionados.

Aqui está como testar a funcionalidade principal do computador:

1. Fazer logon em um computador principal designado com uma conta de usuário para o qual você habilitou o redirecionamento de pasta e/ou perfis de usuário móvel.
2. Se a conta de usuário tiver se conectado ao computador anteriormente, abra uma sessão do Windows PowerShell ou a janela de Prompt de comando como administrador, digite o seguinte comando e, em seguida, aprovar quando solicitado para garantir que as últimas configurações de política de grupo são aplicadas para o computador cliente:
    ```PowerShell
    Gpupdate /force
    ```
3. Abra o Explorador de arquivos.
4. Clique com botão direito uma pasta redirecionada (por exemplo, a pasta de documentos na biblioteca documentos) e, em seguida, selecione **Propriedades**.
5. Selecione a guia **local** e, confirme que o caminho exibe o compartilhamento de arquivo especificado em vez de um caminho local. Para confirmar que o perfil do usuário estiver em roaming, abra o **Painel de controle**, selecione **sistema e segurança**, selecione **sistema**, selecione **Configurações avançadas do sistema**, selecione **as configurações** na seção de perfis de usuário e, em seguida, procure ** Roaming** na coluna **tipo** .
6. Faça logon com a mesma conta de usuário a um computador que não é designado como o computador do usuário principal.
7. Repita as etapas 2 a 5, em vez disso, procurando caminhos locais e um tipo de perfil **Local** .

>[!NOTE]
>Se as pastas foram redirecionadas em um computador antes de você ativar o suporte de computador principal, as pastas permanecerá redirecionadas, a menos que a seguinte configuração é definida na configuração de política de redirecionamento de pasta de cada pasta: **redirecione a pasta local perfil de usuário local quando a política é removida**. Da mesma forma, perfis que anteriormente estavam em roaming em um determinado computador mostrará o **Roaming** nas colunas **tipo** ; No entanto, a coluna **Status** mostrará **Local**.

## Mais informações

- [Implanta o redirecionamento de pasta com arquivos Offline](deploy-folder-redirection.md)
- [Implantar perfis de usuário móvel](deploy-roaming-user-profiles.md)
- [Visão geral de redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](folder-redirection-rup-overview.md)
- [Ao analisar um pouco mais profundamente computador principal do Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)