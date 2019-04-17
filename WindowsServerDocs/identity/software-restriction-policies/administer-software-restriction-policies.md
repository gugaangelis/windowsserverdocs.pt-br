---
title: "Administrar políticas de restrição de Software"
description: "Segurança do Windows Server"
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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="administer-software-restriction-policies"></a>Administrar políticas de restrição de Software

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico para o profissional de TI contém procedimentos como administrar políticas de controle de aplicativo usando o início de políticas de restrição de Software (SRP) com o Windows Server 2008 e Windows Vista.

## <a name="introduction"></a>Introdução
Políticas de restrição de software (SRP) é um recurso baseado em política de grupo que identifica os programas de software executados em computadores em um domínio e controla a capacidade desses programas para execução. Você pode usar políticas de restrição de software para criar uma configuração altamente restrita para computadores, em que você permitir que apenas especificamente identificados aplicativos sejam executados. Esses são integrados ao Microsoft Active Directory Domain Services e política de grupo, mas também podem ser configuradas em computadores autônomos. Para obter mais informações sobre o SRP, consulte o [políticas de restrição de Software](software-restriction-policies.md).

A partir do Windows Server 2008 R2 e Windows 7, Windows AppLocker pode ser usado em vez de ou em conjunto com SRP para uma parte da sua estratégia de controle do aplicativo.

Este tópico contém:

-   [Para abrir as políticas de restrição de Software](#BKMK_Open_SRP)

-   [Para criar novas políticas de restrição de software](#BKMK_Create_SRP)

-   [Para adicionar ou excluir um tipo de arquivo designados](#BKMK_Add_Del)

-   [Para impedir que as políticas de restrição de software aplicadas a administradores locais](#BKMK_Prevent_Admin)

-   [Para alterar o nível de segurança padrão das políticas de restrição de software](#BKMK_Sec_Lvl)

-   [Para aplicar políticas de restrição de software a DLLs](#BKMK_Apply_SRP_DLLs)

Para obter informações sobre como realizar tarefas específicas usando SRP, consulte o seguinte:

-   [Determinar lista permitir negação e inventário de aplicativos para políticas de restrição de Software](determine-allow-deny-list-and-application-inventory-for-software-restriction-policies.md)

-   [Trabalhar com regras de políticas de restrição de Software](work-with-software-restriction-policies-rules.md)

-   [Usar políticas de restrição de Software para ajudar a proteger seu computador contra vírus de Email](use-software-restriction-policies-to-help-protect-your-computer-against-an-email-virus.md)

## <a name="BKMK_Open_SRP"></a>Para abrir as políticas de restrição de Software

-   [Para o computador local](#BKMK_1)

-   [Para um domínio, site ou unidade organizacional e você está em um servidor membro ou estação de trabalho que tenha ingressada em um domínio](#BKMK_2)

-   [Para um domínio ou unidade organizacional e você está em um controlador de domínio ou em uma estação de trabalho que tenha as ferramentas de administração de servidor remoto instalado](#BKMK_3)

-   [Em um site, você está em um controlador de domínio ou em uma estação de trabalho que tenha as ferramentas de administração de servidor remoto instalado](#BKMK_4)

### <a name="BKMK_1"></a>Para o computador local

1.  Abra configurações de segurança Local.

2.  Na árvore de console, clique em **políticas de restrição de Software**.

    **Onde?**

    -   Políticas de restrição de Software/configurações de segurança

> [!NOTE]
> Para executar este procedimento, você deve ser um membro do grupo Administradores no computador local ou ter recebido a autoridade adequada.

### <a name="BKMK_2"></a>Para um domínio, site ou unidade organizacional e você está em um servidor membro ou estação de trabalho que tenha ingressada em um domínio

1.  Abra o Console de gerenciamento Microsoft (MMC).

2.  Sobre o **arquivo** menu, clique em **Adicionar/Remover Snap-in**e clique em **adicionar**.

3.  Clique em **Editor de objeto de política de Grupo Local**e clique em **adicionar**.

4.  Em **Selecionar objeto de diretiva de grupo**, clique em **procurar**.

5.  Em **procurar um objeto de política de grupo**, selecione um objeto de política de grupo (GPO) no domínio apropriado, site ou unidade organizacional- ou crie um novo e clique em **concluir**.

6.  Clique em **fechar**e clique em **Okey**.

7.  Na árvore de console, clique em **políticas de restrição de Software**.

    **Onde?**

    -   *Objeto de política de grupo* [*ComputerName*] configuração de diretiva de computador ou

        Políticas de restrição de Software configurações do usuário Configuration/Windows/configurações de segurança

> [!NOTE]
> Para executar este procedimento, você deve ser um membro do grupo Admins. do domínio.

### <a name="BKMK_3"></a>Para um domínio ou unidade organizacional e você está em um controlador de domínio ou em uma estação de trabalho que tenha as ferramentas de administração de servidor remoto instalado

1.  Abra o Console de gerenciamento de política de grupo.

2.  Na árvore de console, clique com botão direito do objeto de política de grupo (GPO) que você deseja abrir diretivas de restrição de software para.

3.  Clique em **editar** para abrir o GPO que você deseja editar. Você também pode clicar em **nova** para criar um novo GPO e clique em **editar**.

4.  Na árvore de console, clique em **políticas de restrição de Software**.

    **Onde?**

    -   *Objeto de política de grupo* [*ComputerName*] configuração de diretiva de computador ou

        Políticas de restrição de Software configurações do usuário Configuration/Windows/configurações de segurança

> [!NOTE]
> Para executar este procedimento, você deve ser um membro do grupo Admins. do domínio.

### <a name="BKMK_4"></a>Em um site, você está em um controlador de domínio ou em uma estação de trabalho que tenha as ferramentas de administração de servidor remoto instalado

1.  Abra o Console de gerenciamento de política de grupo.

2.  Na árvore de console, clique com botão direito do site que você deseja definir a política de grupo.

    **Onde?**

    -   Serviços e Sites do Active Directory [*nome_do_controlador_do_domínio. nome_do_domínio*] / Sites/Site

3.  Clique em uma entrada **Links de objeto de política de grupo** para selecionar um objeto de política de grupo (GPO) existente e, em seguida, clique em **editar**. Você também pode clicar em **nova** para criar um novo GPO e clique em **editar**.

4.  Na árvore de console, clique em **políticas de restrição de Software**.

    **Onde**

    -   *Objeto de política de grupo* [*ComputerName*] configuração de diretiva de computador ou

        Políticas de restrição de Software configurações do usuário Configuration/Windows/configurações de segurança

> [!NOTE]
> -   Para executar este procedimento, você deve ser um membro do grupo Administradores no computador local ou ter recebido a autoridade adequada. Se o computador tiver ingressado em um domínio, os membros do grupo Admins. do domínio podem ser capazes de executar este procedimento.
> -   Para definir configurações de política que serão aplicadas a computadores, independentemente de quais usuários se conectem a eles, clique em **configuração do computador**.
> -   Para definir configurações de política que serão aplicadas a usuários, independentemente de qual computador façam logon, clique em **configuração do usuário**.

## <a name="BKMK_Create_SRP"></a>Para criar novas políticas de restrição de software

1.  Abra as políticas de restrição de Software.

2.  Sobre o **ação** menu, clique em **novas políticas de restrição de Software**.

> [!WARNING]
> -   São necessárias diversas credenciais administrativas para executar este procedimento, dependendo do seu ambiente:
> 
>     -   Se você criar novas políticas de restrição de software para o computador local: a associação ao grupo local **administradores** grupo, ou equivalente, é o requisito mínimo para concluir este procedimento.
>     -   Se você criar novas políticas de restrição de software para um computador que tenha ingressado em um domínio, os membros do grupo Admins. do domínio podem executar este procedimento.
> -   Se as políticas de restrição de software já tiverem sido criadas para um objeto de política de grupo (GPO), o **novas políticas de restrição de Software** comando não aparece no **ação** menu. Para excluir as políticas de restrição de software que são aplicadas a um GPO, na árvore de console, clique com botão direito **políticas de restrição de Software**e clique em **excluir políticas de restrição de Software**. Quando você exclui as políticas de restrição de software para um GPO, você também excluir todas as regras de políticas de restrição de software para o GPO. Depois de excluir as políticas de restrição de software, você pode criar novas políticas de restrição de software para o GPO.

## <a name="BKMK_Add_Del"></a>Para adicionar ou excluir um tipo de arquivo designados

1.  Abra as políticas de restrição de Software.

2.  No painel de detalhes, clique duas vezes em **tipos de arquivo designados**.

3.  Siga um destes procedimentos:

    -   Para adicionar um tipo de arquivo, em **extensão de nome de arquivo**, digite a extensão de nome de arquivo e clique em **adicionar**.

    -   Para excluir um tipo de arquivo no **tipos de arquivo designados**, clique no tipo de arquivo e clique em **remover**.

> [!NOTE]
> -   São necessárias diversas credenciais administrativas para executar este procedimento, dependendo do ambiente em que você pode adicionar ou excluir um tipo de arquivo designados:
> 
>     -   Se você adicionar ou excluir um tipo de arquivo designados para o computador local: a associação ao grupo local **administradores** grupo, ou equivalente, é o requisito mínimo para concluir este procedimento.
>     -   Se você criar novas políticas de restrição de software para um computador que tenha ingressado em um domínio, os membros do grupo Admins. do domínio podem executar este procedimento.
> -   Ele pode ser necessário criar uma nova configuração de política de restrição de software para o objeto de política de grupo (GPO) se você ainda não tiver feito isso.
> -   A lista de tipos de arquivo designados é compartilhada por todas as regras para configuração do computador e configuração do usuário para um GPO.

## <a name="BKMK_Prevent_Admin"></a>Para impedir que as políticas de restrição de software aplicadas a administradores locais

1.  Abra as políticas de restrição de Software.

2.  No painel de detalhes, clique duas vezes em **imposição**.

3.  Em **aplicar políticas de restrição de software para os usuários a seguir**, clique em **todos os usuários exceto administradores locais**.

> [!WARNING]
> -   A associação ao grupo local **administradores** grupo, ou equivalente, é o requisito mínimo para concluir este procedimento.
> -   Ele pode ser necessário criar uma nova configuração de política de restrição de software para o objeto de política de grupo (GPO) se você ainda não tiver feito isso.
> -   Se é comum que os usuários sejam membros do grupo Administradores local nos computadores em sua organização, não convém habilitar essa opção.
> -   Se você estiver definindo uma configuração de política de restrição de software para o computador local, use este procedimento para impedir que os administradores locais tenham políticas de restrição de software aplicadas a eles. Se você estiver definindo uma configuração de política de restrição de software para a rede, filtre as configurações de política de usuário com base na participação em grupos de segurança por meio da política de grupo.

## <a name="BKMK_Sec_Lvl"></a>Para alterar o nível de segurança padrão das políticas de restrição de software

1.  Abra as políticas de restrição de Software.

2.  No painel de detalhes, clique duas vezes em **níveis de segurança**.

3.  Clique com botão direito o nível de segurança que você deseja definir como padrão e clique em **definir como padrão**.

> [!CAUTION]
> Em determinadas pastas, configuração de segurança padrão nível para **não permitido** pode afetar negativamente o sistema operacional.

> [!NOTE]
> -   São necessárias diversas credenciais administrativas para executar este procedimento, dependendo do ambiente para o qual você alterar o nível de segurança padrão das políticas de restrição de software.
> -   Ele pode ser necessário criar uma nova configuração de política de restrição de software para esse objeto de política de grupo (GPO) se você ainda não tiver feito isso.
> -   No painel de detalhes, o nível de segurança padrão atual é indicado por um círculo preto com uma marca de seleção. Se você clique com botão direito no nível de segurança padrão atual, o **definir como padrão** comando não aparecem no menu.
> -   Regras de políticas de restrição de software são criadas para especificar exceções para o nível de segurança padrão. Quando o nível de segurança padrão é definido como **Unrestricted**, as regras podem especificar o software que não tem permissão para executar. Quando o nível de segurança padrão é definido como **não permitido**, as regras podem especificar o software que pode ser executado.
> -   Durante a instalação, o nível de segurança padrão de diretivas de restrição de software em todos os arquivos no seu sistema é definido como **Unrestricted**.

## <a name="BKMK_Apply_SRP_DLLs"></a>Para aplicar políticas de restrição de software a DLLs

1.  Abra as políticas de restrição de Software.

2.  No painel de detalhes, clique duas vezes em **imposição**.

3.  Em **aplicar políticas de restrição de software para o seguinte**, clique em **todos os arquivos de software**.

> [!NOTE]
> -   Para executar este procedimento, você deve ser um membro do grupo Administradores no computador local ou ter recebido a autoridade adequada. Se o computador tiver ingressado em um domínio, os membros do grupo Admins. do domínio podem ser capazes de executar este procedimento.
> -   Por padrão, as políticas de restrição de software não verificam bibliotecas de vínculo dinâmico (DLLs). A verificação de DLLs pode diminuir o desempenho do sistema, porque as políticas de restrição de software devem ser avaliadas sempre que uma DLL é carregada. No entanto, você pode decidir verificar DLLs, se você estiver preocupado sobre como receber um vírus destinado DLLs. Se o nível de segurança padrão é definido como **não permitido**, e você habilitar DLL verificar, você deve criar a restrição de software regras de políticas que permitem que cada DLL ser executado.


