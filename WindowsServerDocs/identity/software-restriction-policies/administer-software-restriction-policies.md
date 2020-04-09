---
title: Administrar políticas de restrição de software
description: Segurança do Windows Server
ms.prod: windows-server
ms.technology: security-software-restriction-policies
ms.topic: article
ms.assetid: 8cc22093-67d1-47b6-9ddd-4569b6761ce9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 88e745b6951ab27f22cc412ee63f792d30775d14
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855109"
---
# <a name="administer-software-restriction-policies"></a>Administrar políticas de restrição de software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico para o profissional de ti contém procedimentos de como administrar políticas de controle de aplicativos usando políticas de restrição de software (SRP) a partir do Windows Server 2008 e do Windows Vista.

## <a name="introduction"></a>Introdução
A política SRP (Política de Restrição de Software) é um recurso baseado em políticas de grupo que identifica programas de software que são executados em um domínio e que controla a capacidade desses programas de serem executados. Use políticas de restrição de software para criar uma configuração altamente restrita para computadores nos quais você permite que apenas aplicativos especificamente identificados sejam executados. Eles são integrados ao Microsoft Active Directory Domain Services e Política de Grupo, mas também podem ser configurados em computadores autônomos. Para obter mais informações sobre o SRP, consulte as [diretivas de restrição de software](software-restriction-policies.md).

A partir do Windows Server 2008 R2 e do Windows 7, o Windows AppLocker pode ser usado em vez de ou em conjunto com o SRP para uma parte da sua estratégia de controle de aplicativo.

Este tópico contém:

-   [Para abrir as diretivas de restrição de software](#BKMK_Open_SRP)

-   [Para criar novas políticas de restrição de software](#BKMK_Create_SRP)

-   [Para adicionar ou excluir um tipo de arquivo designado](#BKMK_Add_Del)

-   [Para impedir que as diretivas de restrição de software sejam aplicadas a administradores locais](#BKMK_Prevent_Admin)

-   [Para alterar o nível de segurança padrão das políticas de restrição de software](#BKMK_Sec_Lvl)

-   [Para aplicar políticas de restrição de software a DLLs](#BKMK_Apply_SRP_DLLs)

Para obter informações sobre como realizar tarefas específicas usando o SRP, consulte o seguinte:

-   [Determinar lista de permissões de negação e inventário de aplicativos para diretivas de restrição de software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Trabalhar com regras de políticas de restrição de software](work-with-software-restriction-policies-rules.md)

-   [Use as políticas de restrição de software para ajudar a proteger seu computador contra um vírus de email](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="to-open-software-restriction-policies"></a><a name="BKMK_Open_SRP"></a>Para abrir as diretivas de restrição de software

-   [Para seu computador local](#BKMK_1)

-   [Para um domínio, site ou unidade organizacional, e você está em um servidor membro ou em uma estação de trabalho que tenha ingressado em um domínio](#BKMK_2)

-   [Para um domínio ou unidade organizacional, e você está em um controlador de domínio ou em uma estação de trabalho com o Ferramentas de Administração de Servidor Remoto instalado](#BKMK_3)

-   [Para um site, e você está em um controlador de domínio ou em uma estação de trabalho que tem o Ferramentas de Administração de Servidor Remoto instalado](#BKMK_4)

### <a name="for-your-local-computer"></a><a name="BKMK_1"></a>Para seu computador local

1.  Abra as Configurações de Segurança Locais.

2.  Na árvore de console, clique em **diretivas de restrição de software**.

    **Posição?**

    -   Configurações de segurança/políticas de restrição de software

> [!NOTE]
> Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local ou deve ter recebido a autoridade apropriada.

### <a name="for-a-domain-site-or-organizational-unit-and-you-are-on-a-member-server-or-on-a-workstation-that-is-joined-to-a-domain"></a><a name="BKMK_2"></a>Para um domínio, site ou unidade organizacional, e você está em um servidor membro ou em uma estação de trabalho que tenha ingressado em um domínio

1.  Abra o MMC (Console de Gerenciamento Microsoft).

2.  No menu **arquivo** , clique em **Adicionar/remover snap-in**e, em seguida, clique em **Adicionar**.

3.  Clique em **Editor de Objeto de Política de Grupo Local** e clique em **Adicionar**.

4.  Em **Selecionar Objeto de Política de Grupo**, clique em **Procurar**.

5.  Em **procurar um objeto de política de grupo**, selecione um objeto de política de grupo (GPO) no domínio, site ou unidade organizacional apropriado-ou crie um novo e, em seguida, clique em **concluir**.

6.  Clique em **Fechar**e clique em **OK**.

7.  Na árvore de console, clique em **diretivas de restrição de software**.

    **Posição?**

    -   *Política de grupo objeto* [*ComputerName*] configuração do computador/política ou

        Configuração do usuário/configurações do Windows/configurações de segurança/políticas de restrição de software

> [!NOTE]
> Para executar esse procedimento, você deve ser membro do grupo Admins. do domínio.

### <a name="for-a-domain-or-organizational-unit-and-you-are-on-a-domain-controller-or-on-a-workstation-that-has-the-remote-server-administration-tools-installed"></a><a name="BKMK_3"></a>Para um domínio ou unidade organizacional, e você está em um controlador de domínio ou em uma estação de trabalho com o Ferramentas de Administração de Servidor Remoto instalado

1.  Abra Console de Gerenciamento de Política de Grupo.

2.  Na árvore de console, clique com o botão direito do mouse no objeto de Política de Grupo (GPO) para o qual você deseja abrir as diretivas de restrição de software.

3.  Clique em **Editar** para abrir o GPO que quer editar. Você também pode clicar em **Novo** para criar um novo GPO e clique em **Editar**.

4.  Na árvore de console, clique em **diretivas de restrição de software**.

    **Posição?**

    -   *Política de grupo objeto* [*ComputerName*] configuração do computador/política ou

        Configuração do usuário/configurações do Windows/configurações de segurança/políticas de restrição de software

> [!NOTE]
> Para executar esse procedimento, você deve ser membro do grupo Admins. do domínio.

### <a name="for-a-site-and-you-are-on-a-domain-controller-or-on-a-workstation-that-has-the-remote-server-administration-tools-installed"></a><a name="BKMK_4"></a>Para um site, e você está em um controlador de domínio ou em uma estação de trabalho que tem o Ferramentas de Administração de Servidor Remoto instalado

1.  Abra Console de Gerenciamento de Política de Grupo.

2.  Na árvore de console, clique com o botão direito do mouse no site para o qual você deseja definir Política de Grupo.

    **Posição?**

    -   Active Directory sites e serviços [*Domain_Controller_Name. domain_name*]/sites/site

3.  Clique em uma entrada em **política de grupo links de objeto** para selecionar um objeto de política de grupo (GPO) existente e clique em **Editar**. Você também pode clicar em **Novo** para criar um novo GPO e clique em **Editar**.

4.  Na árvore de console, clique em **diretivas de restrição de software**.

    **Posição**

    -   *Política de grupo objeto* [*ComputerName*] configuração do computador/política ou

        Configuração do usuário/configurações do Windows/configurações de segurança/políticas de restrição de software

> [!NOTE]
> -   Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local ou deve ter recebido a autoridade apropriada. Se o computador estiver em um domínio, é possível que os membros do grupo Admins. do Domínio possam executar esse procedimento.
> -   Para definir as configurações de política que serão aplicadas aos computadores, independentemente dos usuários que fizerem logon nelas, clique em **configuração do computador**.
> -   Para definir as configurações de política que serão aplicadas aos usuários, independentemente de em qual computador eles fazem logon, clique em **configuração do usuário**.

## <a name="to-create-new-software-restriction-policies"></a><a name="BKMK_Create_SRP"></a>Para criar novas políticas de restrição de software

1.  Abra Políticas de Restrição de Software.

2.  No menu **Ação**, clique em **Novas Políticas de Restrição de Software**.

> [!WARNING]
> -   São necessárias diferentes credenciais administrativas para executar esse procedimento, dependendo do seu ambiente:
> 
>     -   Se você criar novas diretivas de restrição de software para seu computador local: a associação no grupo local de **Administradores** , ou equivalente, é o requisito mínimo necessário para concluir este procedimento.
>     -   Se você criar novas políticas de restrição de software para um computador que ingressou em um domínio, os membros do grupo Administradores de Domínio poderão executar esse procedimento.
> -   Se as políticas de restrição de software já tiverem sido criadas para um GPO (Objeto de Política de Grupo), o comando **Novas Políticas de Restrição de Software** não será exibido no menu **Ação**. Para excluir as políticas de restrição de software aplicadas a um GPO, na árvore de console, clique com o botão direito do mouse em **Políticas de Restrição de Software** e clique em **Excluir Políticas de Restrição de Software**. Ao excluir as políticas de restrição de software para um GPO, você também exclui todas as regras de diretivas de restrição de software desse GPO. Depois de criar as políticas de restrição de software, você pode criar novas políticas de restrição de software para esse GPO.

## <a name="to-add-or-delete-a-designated-file-type"></a><a name="BKMK_Add_Del"></a>Para adicionar ou excluir um tipo de arquivo designado

1.  Abra Políticas de Restrição de Software.

2.  No painel de detalhes, clique duas vezes em **Tipos de Arquivo Designados**.

3.  Siga um destes procedimentos:

    -   Para adicionar um tipo de arquivo, em **Extensão de nome de arquivo**, digite a extensão do nome do arquivo e clique em **Adicionar**.

    -   Para excluir um tipo de arquivo, em **Tipos de arquivo designados**, clique no tipo de arquivo e clique em **Remover**.

> [!NOTE]
> -   São necessárias diferentes credenciais administrativas para executar esse procedimento, dependendo do ambiente em que você adiciona ou exclui um tipo de arquivo designado:
> 
>     -   Se você adicionar ou excluir um tipo de arquivo designado para seu computador local: a associação no grupo **Administradores** local ou equivalente é o mínimo necessário para concluir este procedimento.
>     -   Se você criar novas políticas de restrição de software para um computador que ingressou em um domínio, os membros do grupo Administradores de Domínio poderão executar esse procedimento.
> -   Talvez seja necessário criar uma nova configuração de política de restrição de software para o GPO (Objeto de Política de Grupo), se você ainda não tiver feito isso.
> -   A lista de tipos de arquivo designados é compartilhada por todas as regras da configuração do computador e da configuração do usuário para um GPO.

## <a name="to-prevent-software-restriction-policies-from-applying-to-local-administrators"></a><a name="BKMK_Prevent_Admin"></a>Para impedir que as diretivas de restrição de software sejam aplicadas a administradores locais

1.  Abra Políticas de Restrição de Software.

2.  No painel de detalhes, clique duas vezes em **Imposição**.

3.  Em **Aplicar políticas de restrição de software aos seguintes usuários**, clique em **Todos os usuários, exceto administradores locais**.

> [!WARNING]
> -   A associação no grupo **Administradores** local, ou equivalente, é o mínimo necessário para concluir este procedimento.
> -   Talvez seja necessário criar uma nova configuração de política de restrição de software para o GPO (Objeto de Política de Grupo), se você ainda não tiver feito isso.
> -   Se for comum que os usuários sejam membros do grupo local de administradores nos computadores de sua organização, convém não habilitar essa opção.
> -   Se você estiver definindo uma configuração de política de restrição de software para o computador local, use esse procedimento para impedir que administradores locais tenham as políticas de restrição de software aplicadas a eles. Se você estiver definindo uma configuração de diretiva de restrição de software para sua rede, filtre as configurações de política de usuário com base na associação em grupos de segurança por meio de Política de Grupo.

## <a name="to-change-the-default-security-level-of-software-restriction-policies"></a><a name="BKMK_Sec_Lvl"></a>Para alterar o nível de segurança padrão das políticas de restrição de software

1.  Abra Políticas de Restrição de Software.

2.  No painel de detalhes, clique duas vezes em **Níveis de Segurança**.

3.  Clique com o botão direito do mouse no nível de segurança que você quer definir como padrão e clique em **Definir como padrão**.

> [!CAUTION]
> Em determinados diretórios, a configuração do nível de segurança como **Não Permitido** pode prejudicar o sistema operacional.

> [!NOTE]
> -   São necessárias diferentes credenciais administrativas para executar esse procedimento, dependendo do ambiente para o qual você altera o nível de segurança padrão das políticas de restrição de software.
> -   Talvez seja necessário criar uma nova configuração de política de restrição de software para o GPO (Objeto de Política de Grupo), se você ainda não tiver feito isso.
> -   No painel de detalhes, o nível de segurança padrão atual é indicado por um círculo preto com uma marca de seleção nele. Se você clicar com o botão direito do mouse no nível de segurança padrão, o comando **Definir como padrão** não será exibido no menu.
> -   As regras de diretivas de restrição de software são criadas para especificar exceções para o nível de segurança padrão. Quando o nível de segurança padrão for definido como **Irrestrito**, as regras poderão especificar software que não têm permissão para serem executados. Quando o nível de segurança padrão for definido como **Não Permitido**, as regras poderão especificar software que têm permissão para serem executados.
> -   Na instalação, o nível de segurança padrão das políticas de restrição de software em todos os arquivos no sistema é definido como **Irrestrito**.

## <a name="to-apply-software-restriction-policies-to-dlls"></a><a name="BKMK_Apply_SRP_DLLs"></a>Para aplicar políticas de restrição de software a DLLs

1.  Abra Políticas de Restrição de Software.

2.  No painel de detalhes, clique duas vezes em **Imposição**.

3.  Em **Aplicar políticas de restrição de software a**, clique em **Todos os arquivos de software**.

> [!NOTE]
> -   Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local ou deve ter recebido a autoridade apropriada. Se o computador estiver em um domínio, é possível que os membros do grupo Admins. do Domínio possam executar esse procedimento.
> -   Por padrão, as políticas de restrição de software não verificam DLLs (bibliotecas de links dinâmicos). A verificação de DLLs pode diminuir o desempenho do sistema, porque as políticas de restrição de software devem ser avaliadas sempre que uma DLL é carregada. Entretanto, você poderá optar por verificar as DLLs se estiver preocupado que receberá um vírus direcionado a DLLs. Se o nível de segurança padrão estiver definido como não **permitido**e você habilitar a verificação de dll, você deverá criar regras de diretivas de restrição de software que permitam a execução de cada DLL.


