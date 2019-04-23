---
title: Administrar políticas de restrição de software
description: Segurança do Windows Server
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: security-software-restriction-policies
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8cc22093-67d1-47b6-9ddd-4569b6761ce9
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/12/2016
ms.openlocfilehash: 67ef99c95f10585ab72dee4bdf6512e715d13f4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863587"
---
# <a name="administer-software-restriction-policies"></a>Administrar políticas de restrição de software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico para profissionais de TI contém procedimentos como administrar políticas de controle de aplicativo usando políticas de restrição de Software (SRP) que começa com o Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introdução
A política SRP (Política de Restrição de Software) é um recurso baseado em políticas de grupo que identifica programas de software que são executados em um domínio e que controla a capacidade desses programas de serem executados. Use políticas de restrição de software para criar uma configuração altamente restrita para computadores nos quais você permite que apenas aplicativos especificamente identificados sejam executados. Elas são integradas ao Microsoft Active Directory Domain Services e a diretiva de grupo, mas também podem ser configuradas em computadores autônomos. Para obter mais informações sobre o SRP, consulte o [diretivas de restrição de Software](software-restriction-policies.md).

Começando com o Windows Server 2008 R2 e Windows 7, Windows AppLocker pode ser usado em vez de ou em conjunto com as SRP por uma parte de sua estratégia de controle de aplicativo.

Este tópico contém:

-   [Para abrir as diretivas de restrição de Software](#BKMK_Open_SRP)

-   [Para criar novas políticas de restrição de software](#BKMK_Create_SRP)

-   [Para adicionar ou excluir um tipo de arquivo designado](#BKMK_Add_Del)

-   [Para impedir que as políticas de restrição de software aplicadas a administradores locais](#BKMK_Prevent_Admin)

-   [Para alterar o nível de segurança padrão de diretivas de restrição de software](#BKMK_Sec_Lvl)

-   [Para aplicar políticas de restrição de software a DLLs](#BKMK_Apply_SRP_DLLs)

Para obter informações sobre como realizar tarefas específicas usando o SRP, consulte o seguinte:

-   [Determinar a lista de permissão-restrição e inventário de aplicativos para políticas de restrição de Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Trabalhar com regras de políticas de restrição de Software](work-with-software-restriction-policies-rules.md)

-   [Usar políticas de restrição de Software para ajudar a proteger seu computador contra um vírus de Email](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Para abrir as diretivas de restrição de Software

-   [Para o computador local](#BKMK_1)

-   [Para um domínio, site, ou unidade organizacional e você está em um servidor membro ou estação de trabalho que tenha ingressada em um domínio](#BKMK_2)

-   [Para um domínio ou unidade organizacional e estão em um controlador de domínio ou em uma estação de trabalho que tenha as ferramentas de administração de servidor remoto instalado](#BKMK_3)

-   [Em um site, você está em um controlador de domínio ou em uma estação de trabalho que tenha as ferramentas de administração de servidor remoto instalado](#BKMK_4)

### <a name="BKMK_1"></a>Para o computador local

1.  Abra as Configurações de Segurança Locais.

2.  Na árvore de console, clique em **diretivas de restrição de Software**.

    **Onde?**

    -   Políticas de restrição de Software/configurações de segurança

> [!NOTE]
> Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local ou deve ter recebido a autoridade apropriada.

### <a name="BKMK_2"></a>Para um domínio, site, ou unidade organizacional e você está em um servidor membro ou estação de trabalho que tenha ingressada em um domínio

1.  Abra o MMC (Console de Gerenciamento Microsoft).

2.  Sobre o **arquivo** menu, clique em **Adicionar/Remover Snap-in**e, em seguida, clique em **adicionar**.

3.  Clique em **Editor de Objeto de Política de Grupo Local** e clique em **Adicionar**.

4.  Em **Selecionar Objeto de Política de Grupo**, clique em **Procurar**.

5.  Na **procurar um objeto de política de grupo**, selecione um objeto de diretiva de grupo (GPO) no domínio apropriado, no site ou na unidade organizacional- ou crie um novo e, em seguida, clique em **concluir**.

6.  Clique em **Fechar**e clique em **OK**.

7.  Na árvore de console, clique em **diretivas de restrição de Software**.

    **Onde?**

    -   *Objeto de diretiva de grupo* [*ComputerName*] configuração de diretiva de computador ou

        Políticas de restrição de configurações de Software do usuário Configuration/Windows/configurações de segurança

> [!NOTE]
> Para executar este procedimento, você deve ser um membro do grupo Admins. do domínio.

### <a name="BKMK_3"></a>Para um domínio ou unidade organizacional e estão em um controlador de domínio ou em uma estação de trabalho que tenha as ferramentas de administração de servidor remoto instalado

1.  Abra o Console de gerenciamento de diretiva de grupo.

2.  Na árvore de console, clique com botão direito do grupo de política de GPO (objeto) que você deseja abrir diretivas de restrição de software para.

3.  Clique em **Editar** para abrir o GPO que quer editar. Você também pode clicar em **Novo** para criar um novo GPO e clique em **Editar**.

4.  Na árvore de console, clique em **diretivas de restrição de Software**.

    **Onde?**

    -   *Objeto de diretiva de grupo* [*ComputerName*] configuração de diretiva de computador ou

        Políticas de restrição de configurações de Software do usuário Configuration/Windows/configurações de segurança

> [!NOTE]
> Para executar este procedimento, você deve ser um membro do grupo Admins. do domínio.

### <a name="BKMK_4"></a>Em um site, você está em um controlador de domínio ou em uma estação de trabalho que tenha as ferramentas de administração de servidor remoto instalado

1.  Abra o Console de gerenciamento de diretiva de grupo.

2.  Na árvore de console, clique com botão direito no site que você deseja definir a política de grupo para.

    **Onde?**

    -   Serviços e Sites do Active Directory [*nome_do_controlador_do_domínio. nome_do_domínio*] / Sites/Site

3.  Clique em uma entrada **vínculos de objeto de diretiva de grupo** para selecionar um objeto de política de grupo (GPO) existente e, em seguida, clique em **editar**. Você também pode clicar em **Novo** para criar um novo GPO e clique em **Editar**.

4.  Na árvore de console, clique em **diretivas de restrição de Software**.

    **Where**

    -   *Objeto de diretiva de grupo* [*ComputerName*] configuração de diretiva de computador ou

        Políticas de restrição de configurações de Software do usuário Configuration/Windows/configurações de segurança

> [!NOTE]
> -   Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local ou deve ter recebido a autoridade apropriada. Se o computador fizer parte de um domínio, é possível que os membros do grupo Administradores de domínio possam executar esse procedimento.
> -   Para definir configurações de política que serão aplicadas a computadores, independentemente dos usuários que fizerem logon no-los, clique em **configuração do computador**.
> -   Para definir configurações de política que serão aplicadas a usuários, independentemente do computador que eles fizerem logon, clique em **configuração do usuário**.

## <a name="BKMK_Create_SRP"></a>Para criar novas políticas de restrição de software

1.  Abra Políticas de Restrição de Software.

2.  No menu **Ação**, clique em **Novas Políticas de Restrição de Software**.

> [!WARNING]
> -   São necessárias diferentes credenciais administrativas para executar esse procedimento, dependendo do seu ambiente:
> 
>     -   Se você criar novas políticas de restrição de software para o computador local: A associação no grupo local **Administradores**, ou equivalente, é o mínimo necessário para concluir esse procedimento.
>     -   Se você criar novas políticas de restrição de software para um computador que ingressou em um domínio, os membros do grupo Administradores de Domínio poderão executar esse procedimento.
> -   Se as políticas de restrição de software já tiverem sido criadas para um GPO (Objeto de Política de Grupo), o comando **Novas Políticas de Restrição de Software** não será exibido no menu **Ação**. Para excluir as políticas de restrição de software aplicadas a um GPO, na árvore de console, clique com o botão direito do mouse em **Políticas de Restrição de Software** e clique em **Excluir Políticas de Restrição de Software**. Quando você exclui as políticas de restrição de software para um GPO, você também excluir todas as regras de políticas de restrição de software para o GPO. Depois de criar as políticas de restrição de software, você pode criar novas políticas de restrição de software para esse GPO.

## <a name="BKMK_Add_Del"></a>Para adicionar ou excluir um tipo de arquivo designado

1.  Abra Políticas de Restrição de Software.

2.  No painel de detalhes, clique duas vezes em **Tipos de Arquivo Designados**.

3.  Siga um destes procedimentos:

    -   Para adicionar um tipo de arquivo, em **Extensão de nome de arquivo**, digite a extensão do nome do arquivo e clique em **Adicionar**.

    -   Para excluir um tipo de arquivo, em **Tipos de arquivo designados**, clique no tipo de arquivo e clique em **Remover**.

> [!NOTE]
> -   São necessárias diferentes credenciais administrativas para executar esse procedimento, dependendo do ambiente em que você adiciona ou exclui um tipo de arquivo designado:
> 
>     -   Se você adicionar ou excluir um tipo de arquivo designado para o computador local: A associação no grupo local **Administradores**, ou equivalente, é o mínimo necessário para concluir esse procedimento.
>     -   Se você criar novas políticas de restrição de software para um computador que ingressou em um domínio, os membros do grupo Administradores de Domínio poderão executar esse procedimento.
> -   Talvez seja necessário criar uma nova configuração de política de restrição de software para o GPO (Objeto de Política de Grupo), se você ainda não tiver feito isso.
> -   A lista de tipos de arquivo designados é compartilhada por todas as regras de configuração do computador e configuração do usuário para um GPO.

## <a name="BKMK_Prevent_Admin"></a>Para impedir que as políticas de restrição de software aplicadas a administradores locais

1.  Abra Políticas de Restrição de Software.

2.  No painel de detalhes, clique duas vezes em **Imposição**.

3.  Em **Aplicar políticas de restrição de software aos seguintes usuários**, clique em **Todos os usuários, exceto administradores locais**.

> [!WARNING]
> -   A associação no grupo local **Administradores**, ou equivalente, é o mínimo necessário para concluir esse procedimento.
> -   Talvez seja necessário criar uma nova configuração de política de restrição de software para o GPO (Objeto de Política de Grupo), se você ainda não tiver feito isso.
> -   Se for comum que os usuários sejam membros do grupo local de administradores nos computadores de sua organização, convém não habilitar essa opção.
> -   Se você estiver definindo uma configuração de política de restrição de software para o computador local, use esse procedimento para impedir que administradores locais tenham as políticas de restrição de software aplicadas a eles. Se você estiver definindo uma configuração de diretiva de restrição de software para sua rede, filtre as configurações de política de usuário com base na associação em grupos de segurança por meio da diretiva de grupo.

## <a name="BKMK_Sec_Lvl"></a>Para alterar o nível de segurança padrão de diretivas de restrição de software

1.  Abra Políticas de Restrição de Software.

2.  No painel de detalhes, clique duas vezes em **Níveis de Segurança**.

3.  Clique com o botão direito do mouse no nível de segurança que você quer definir como padrão e clique em **Definir como padrão**.

> [!CAUTION]
> Em determinados diretórios, a configuração do nível de segurança como **Não Permitido** pode prejudicar o sistema operacional.

> [!NOTE]
> -   São necessárias diferentes credenciais administrativas para executar esse procedimento, dependendo do ambiente para o qual você altera o nível de segurança padrão das políticas de restrição de software.
> -   Talvez seja necessário criar uma nova configuração de política de restrição de software para o GPO (Objeto de Política de Grupo), se você ainda não tiver feito isso.
> -   No painel de detalhes, o nível de segurança padrão atual é indicado por um círculo preto com uma marca de seleção nele. Se você clicar com o botão direito do mouse no nível de segurança padrão, o comando **Definir como padrão** não será exibido no menu.
> -   Regras de políticas de restrição de software são criadas para especificar exceções para o nível de segurança padrão. Quando o nível de segurança padrão for definido como **Irrestrito**, as regras poderão especificar software que não têm permissão para serem executados. Quando o nível de segurança padrão for definido como **Não Permitido**, as regras poderão especificar software que têm permissão para serem executados.
> -   Na instalação, o nível de segurança padrão das políticas de restrição de software em todos os arquivos no sistema é definido como **Irrestrito**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Para aplicar políticas de restrição de software a DLLs

1.  Abra Políticas de Restrição de Software.

2.  No painel de detalhes, clique duas vezes em **Imposição**.

3.  Em **Aplicar políticas de restrição de software a**, clique em **Todos os arquivos de software**.

> [!NOTE]
> -   Para executar esse procedimento, você deve ser membro do grupo Administradores no computador local ou deve ter recebido a autoridade apropriada. Se o computador fizer parte de um domínio, é possível que os membros do grupo Administradores de domínio possam executar esse procedimento.
> -   Por padrão, as políticas de restrição de software não verificam DLLs (bibliotecas de links dinâmicos). A verificação de DLLs pode diminuir o desempenho do sistema, porque as políticas de restrição de software devem ser avaliadas sempre que uma DLL é carregada. Entretanto, você poderá optar por verificar as DLLs se estiver preocupado que receberá um vírus direcionado a DLLs. Se o nível de segurança padrão for definido como **não permitido**, e você habilitar a verificação de DLLs, você deve criar a restrição de software regras de políticas que permitem que cada DLL seja executada.


