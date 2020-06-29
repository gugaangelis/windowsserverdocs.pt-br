---
title: Gerenciar contas Online para usuários do Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
author: nnamuhcs
ms.author: daveba
ms.openlocfilehash: 893d22182b61dda4632550fab1c0a1ebf92c2d89
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85470661"
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>Gerenciar contas Online para usuários do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Ao integrar seu servidor do Windows Server Essentials com o Microsoft Office 365, você pode gerenciar suas contas online junto com as contas de usuário do painel. Neste tópico, você descobrirá o que pode obter gerenciando as contas dos usuários do Microsoft Online Services no painel, como criar e gerenciar contas online no painel e como gerenciar endereços de email e grupos de distribuição para o Exchange Online por meio do painel.


> [!NOTE]
>  Para gerenciar suas contas do Microsoft Online Services no Windows Server Essentials, você deve integrar seu servidor com o Office 365. Para obter instruções, consulte [gerenciar o Office 365](Manage-Office-365-in-Windows-Server-Essentials.md).

> [!IMPORTANT]
>  Se você estiver gerenciando contas online no Windows Server Essentials, está acostumado a ver as contas do Microsoft Online Services mencionadas como *contas do Office 365*. No painel do Windows Server Essentials, os rótulos foram alterados para *contas do Microsoft Online Services*ou *contas online da Microsoft* para serem curtos. As contas e os procedimentos são iguais, somente os rótulos foram alterados. A maioria dos procedimentos neste tópico usam o termo *conta online*.

## <a name="in-this-topic"></a>Neste tópico

-   [Por que devo gerenciar minhas contas online do Painel?](#BKMK_WhyManageOnlineAccounts)

-   [Criar contas online](#BKMK_SECTION_CreateOnlineAccounts)

-   [Gerenciar contas online](#BKMK_SECTION_ManageOnlineAccounts)

-   [Gerenciar endereços de email para o Exchange Online](#BKMK_SECTION_ManageEmailAddresses)

-   [Gerenciar grupos de distribuição do Exchange Online](#BKMK_SECTION_ManageDistributionGroups)

##  <a name="why-should-i-manage-my-online-accounts-from-the-dashboard"></a><a name="BKMK_WhyManageOnlineAccounts"></a>Por que devo gerenciar minhas contas online a partir do painel?
 Quando você usa o painel para atribuir uma conta do Microsoft Online Services a uma conta de usuário, as senhas de conta são sincronizadas automaticamente e você pode manter as duas contas juntas em todo o ciclo de vida da conta de usuário.

 Ele é conveniente para o usuário, que pode usar a mesma senha para acessar recursos no servidor e no Office 365. E você pode aplicar os mesmos requisitos de senha para acesso aos recursos no Office 365 necessários para seus recursos internos.

### <a name="how-does-password-synchronization-work"></a>Como funciona a sincronização de senha?
 Quando você usa o painel para atribuir uma conta do Microsoft Online Services a uma conta de usuário, a senha da conta de usuário é sincronizada automaticamente com a conta do usuário online. Isso significa que um usuário precisa apenas de uma única senha para acessar os recursos no servidor e no Office 365. Além disso, você pode usar o mesmo nome para a conta de usuário e a ID do usuário online.

 A sincronização de senha ocorre imediatamente e automaticamente quando um usuário altera a senha da conta de usuário de um computador associado ao domínio ou usando o Acesso Remoto via Web.

> [!IMPORTANT]
>  Se o Office 365 estiver integrado ao Windows Server Essentials, os usuários não devem alterar a senha de sua conta do Microsoft Online no portal do Office 365. Isso interromperá a sincronização de senha.

### <a name="simplified-account-creation"></a>Criação de contas simplificada
 Há outra vantagem quando você cria suas contas online iniciais no painel. Você pode criar contas online para todos os usuários com uma única ação. Por outro lado, se seus funcionários já estiverem usando o Office 365 e você estiver configurando um novo servidor do Windows Server Essentials, você poderá criar todas as contas de usuário a partir das contas online com uma única ação. Para obter mais informações, consulte [Criar contas online](#BKMK_SECTION_CreateOnlineAccounts).

### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>Gerenciar endereços de email e grupos de distribuição do Painel
 Você poderá gerenciar seus endereços de email e grupos de distribuição do Exchange Online do Painel. E você pode usar o domínio da Internet da sua organização nos endereços de email. Você pode fazer tudo isso no painel, sem entrar no Office 365. (Você precisa estar usando o Windows Server Essentials para gerenciar grupos de distribuição do painel. Não há suporte para esse recurso no Windows Server Essentials.) Para obter mais informações, consulte [gerenciar endereços de email para o Exchange Online](#BKMK_SECTION_ManageEmailAddresses) e [gerenciar grupos de distribuição para o Exchange Online](#BKMK_SECTION_ManageDistributionGroups).

### <a name="manage-the-user-account-and-online-account-together"></a>Gerenciar contas de usuário e online em conjunto
 E você pode gerenciar uma conta online junto com a conta de usuário em todo o ciclo de vida da conta. Se você desativar a conta de usuário, a conta online também será desativada no Microsoft Online Services. Se você remover uma conta de usuário, a conta online também será removida. Para obter mais informações, consulte [Gerenciar contas online](#BKMK_SECTION_ManageOnlineAccounts).

##  <a name="create-online-accounts"></a><a name="BKMK_SECTION_CreateOnlineAccounts"></a>Criar contas online
 Depois de integrar seu servidor com o Office 365, ele é a sua vantagem de criar contas do Microsoft Online Services para seus usuários no painel. Você tem muita flexibilidade na criação de contas online. Se você tiver uma nova assinatura do Office 365, poderá criar contas online em massa para todos os seus usuários. Se você já tiver criado suas contas online no Office 365, Don t me preocupam. Se você configurar um novo servidor, poderá criar suas contas de usuário no servidor importando as contas online. E você pode atribuir uma conta online nova ou existente ao criar uma conta de usuário individual ou ao adicionar uma conta online a uma conta de usuário existente.

 **Requisitos de licença** Você precisará de uma licença de usuário para cada conta online que criar. Verifique a página do **office 365** no painel para ver quantas licenças de usuário estão disponíveis por meio de sua assinatura do Office 365. Se precisar adicionar mais licenças de usuário, você poderá abrir sua assinatura do Office 365 no Office 365 para fazer isso.

 **Acompanhamento para usuários** Depois de adicionar uma conta online para uma conta de usuário, o usuário precisará alterar a senha de sua conta de usuário na próxima vez que entrar. A nova senha é sincronizada imediatamente com sua nova conta online. Depois disso, eles podem usar a senha para entrar no Office 365 com sua ID online.

> [!IMPORTANT]
>  Enfatize para os usuários que eles nunca devem alterar sua senha de conta online no Office 365. A senha será alterada automaticamente sempre que eles alterarem a senha da conta de usuário. Se eles alterarem a senha online no Office 365, a sincronização de senha será interrompida.

 Use os procedimentos nesta seção para:

-   [Criar contas online em massa para suas contas de usuário existente](#BKMK_ToBulkCreateOnlineAccounts)

-   [Importar contas de usuário de suas contas de serviços Online da Microsoft](#BKMK_ToImportUserAccounts)

-   [Criar uma nova conta de usuário com uma conta online](#BKMK_ToCreateaNewUserAccount)

-   [Atribuir uma conta online a uma conta de usuário](#BKMK_ToAssignAnOnlineAccount)

> [!NOTE]
>  Se você estiver usando o Windows Server Essentials, verá a *conta do Office 365* em vez da *conta do Microsoft Online Services* em todos esses procedimentos. O processo é o mesmo, mas a terminologia mudou no Windows Server Essentials.

###  <a name="to-bulk-create-online-accounts-for-your-existing-user-accounts"></a><a name="BKMK_ToBulkCreateOnlineAccounts"></a>Para criar contas online em massa para suas contas de usuário existentes

1.  Entre no servidor como administrador e abra o painel do Windows Server Essentials.

2.  No Painel, abra a página **Usuários**.

3.  Em **Tarefas de usuários**, clique em **Adicionar contas online da Microsoft**.

     A página **Adicionar contas dos serviços Online da Microsoft** do assistente exibe todas as contas de usuário que não têm uma conta online da Microsoft. Todas as contas são selecionadas por padrão e o nome de usuário é sugerido para a ID online da Microsoft. Se você tiver vinculado um domínio de Internet personalizado à sua assinatura do Office 365, esse domínio será usado por padrão.

4.  Na página **Adicionar contas dos serviços Online da Microsoft** , examine as contas que serão criadas. Por exemplo, verifique os usuários que já tem uma conta online com uma ID online diferente e verifique se o domínio que você deseja usar em endereços de email está selecionado. Quando terminar de fazer quaisquer alterações necessárias, clique em **Avançar**.

5.  Na página **atribuir licenças do Microsoft Online Services** , selecione serviços do Office 365 que seus usuários usarão. Quando você clicar em **Avançar**, começará a criação de contas.

    > [!NOTE]
    >  Você deve ter uma licença de serviço no Office 365 para cada conta online. Você pode verificar as licenças disponíveis e, se necessário, abrir sua assinatura para adicionar licenças na página **Office 365** no Painel.

6.  Notifique os usuários de que agora eles têm uma conta online da Microsoft. Eles devem alterar sua senha de conta de usuário de rede antes que possam entrar no Office 365. Para obter instruções, consulte [Para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).

###  <a name="to-begin-using-a-new-microsoft-online-account"></a><a name="BKMK_ToBeginUsingAnOnlineAccount"></a>Para começar a usar uma nova conta do Microsoft Online

1.  Entre em seu computador com a conta de usuário de rede.

2.  Altere a senha da conta do usuário. Para começar, pressione Ctrl + Alt + Delete e clique em **Alterar uma senha**.

     Quando você alterar sua senha, a senha será sincronizada com sua nova conta online. Agora você pode usar a mesma senha para entrar no Office 365.

3.  [Entre no Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) usando sua nova ID online e sua senha de conta do usuário.

    > [!IMPORTANT]
    >  Não altere a senha da sua conta online no Office 365. Isso interromperá a sincronização de senha. Sua senha online será atualizada sempre que você alterar a senha para a conta de usuário de rede.

###  <a name="to-import-user-accounts-from-your-existing-online-accounts"></a><a name="BKMK_ToImportUserAccounts"></a>Para importar contas de usuário de suas contas online existentes

1.  No Painel, abra a página **Usuários**.

2.  No **Tarefas de usuários**, clique em **Importar contas do Office 365**.

     A próxima página exibe todas as contas online da sua assinatura do Office 365 que não têm uma conta de usuário no servidor. Todas as contas são selecionadas por padrão e a ID online é sugerida como nome de usuário.

3.  Para criar as contas de usuário:

    1.  Faça as alterações que são necessários para as contas de usuário propostas.

    2.  Opcionalmente, clique no link para exibir as senhas temporárias que serão atribuídas às contas de usuário. Você precisará fornecer a seus usuários senhas temporárias juntamente com seu novo nome de conta.

         (Depois de criar as contas, você encontrará essas senhas listadas neste arquivo: *systemdrive*\Users \\ *Office365admin* \\ *NewServerUser*. txt, em que *Office365admin* é a conta de rede usada para administrar o Office 365 no servidor e *NewServerUser* é o novo nome de conta de usuário.)

    3.  Clique em **Avançar** para criar as contas de usuário.

4.  Permita que os usuários saibam suas novas contas de usuário e as senhas temporárias que eles usarão para entrar no servidor pela primeira vez no œ e que terão que alterar a senha depois que entrarem. Para obter instruções para os usuários, consulte [Para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).

     Certifique-se de que elas saibam que as senhas de sua conta online serão sincronizadas com sua conta de usuário no futuro e não deverão alterar sua senha online no Office 365.

###  <a name="to-create-a-new-user-account-with-an-online-account-assigned-to-it"></a><a name="BKMK_ToCreateaNewUserAccount"></a>Para criar uma nova conta de usuário com uma conta online atribuída a ela

1.  No Painel, clique em **Usuários**.

2.  Em **Tarefas de usuários**, clique em **Adicionar uma conta de usuário**. O Assistente de Adição de Conta de Usuário é exibido.

3.  Siga as instruções para criar a conta de usuário.

4.  Na página **Atribuir uma conta de serviços Online da Microsoft**, crie uma nova conta online para o usuário ou atribua uma conta online existente:

    -   Para criar uma nova conta online, clique em **Criar uma nova conta de serviços Online da Microsoft e atribuí-la a essa conta de usuário**, e digite um nome para a conta de serviços Online da Microsoft (por padrão, o nome de usuário é usado para a ID online). Em seguida, clique em **Próximo**.

    -   Para atribuir uma conta online ao Microsoft existente, clique em **Atribuir uma conta de serviços Online da Microsoft a esta conta de usuário** e selecione uma conta existente na lista suspensa. Em seguida, clique em **Próximo**.

    > [!NOTE]
    >  No Windows Server Essentials, as contas do Microsoft Online Services são chamadas de contas do Office 365 em assistentes e rótulos de painel.

5.  Siga as instruções para concluir o assistente.

6.  Notifique o usuário de que ele deverá alterar a senha da conta de usuário antes de poder entrar no Office 365 com a nova conta online. Para obter instruções, consulte [Para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).

#### <a name="to-assign-an-online-account-to-a-user-account"></a><a name="BKMK_ToAssignAnOnlineAccount"></a>Para atribuir uma conta online a uma conta de usuário

1.  No Painel, clique em **Usuários**.

2.  Clique com botão direito na lista de contas de usuário e clique em **Atribuir uma conta da Microsoft online**. O Assistente de atribuição de uma conta de serviços Online da Microsoft é exibido.

3.  Atribua uma conta online existente ou crie um novo usuário. A ID online padrão para uma nova conta é o nome de usuário. Em seguida, clique em **Avançar** para adicionar a conta online para a conta de usuário.

4.  Examine as informações na última página do assistente e clique em **Fechar**.

5.  Notifique o usuário de que ele deverá alterar a senha da conta de usuário antes de poder entrar no Office 365 com a nova conta online. Para obter instruções, consulte [Para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).

##  <a name="manage-online-accounts"></a><a name="BKMK_SECTION_ManageOnlineAccounts"></a>Gerenciar contas online
 Quando você adiciona uma conta online a uma conta de usuário no Windows Server Essentials, você pode gerenciar ambas as contas em todo o ciclo de vida da conta.

###  <a name="understanding-the-online-account-status"></a><a name="BKMK_UnderstandingAccountStatus"></a>Noções básicas sobre o status da conta online
 Quando você atribuir uma conta de serviços Online da Microsoft a uma conta de usuário, o endereço de email para a conta aparece na coluna **Conta online da Microsoft** na página **Usuários** do Painel. (No Windows Server Essentials, o rótulo da coluna é **conta do Office 365**.)

-   Um ícone azul ao lado de um endereço de email indica que a conta online está ativa. Ou seja, a conta tem uma licença atual do Office 365 e o usuário pode usar a ID online para entrar no Office 365.

-   Um ícone sombreado ao lado do endereço de email indica que a conta online está inativa œ porque a licença não está mais ativa ou a conta online não foi atribuída. Quando você cancelar a atribuição de uma conta do usuário online, a licença será removida e o usuário será impedido de entrar no Office 365 usando a conta. No entanto, o servidor mantém o mapeamento entre o nome da conta de usuário e o endereço de email do Office 365.

###  <a name="restrict-access-to-an-online-account"></a><a name="BKMK_UnassignOnlineAccount"></a>Restringir o acesso a uma conta online
 O que fazer se um usuário sair da sua organização ou se você quiser restringir o acesso do usuário aos serviços do Office 365? Se você gerencia contas online de seus usuários junto com suas contas de usuário no Windows Server Essentials, você tem três opções:

-   **Cancelar a atribuição da conta online** ? Se desejar impedir que um usuário use o Office 365 sem impedir o acesso aos recursos no servidor, você deverá cancelar a atribuição da conta online. A licença do Office 365 será liberada e o usuário será impedido de entrar no Office 365. No entanto, o servidor mantém o mapeamento entre o nome da conta de usuário e o endereço de email do Office 365. Para obter instruções, consulte [para cancelar a atribuição de uma conta online de uma conta de usuário](#BKMK_ToUnassignAnOnlineAccount).

-   **Desativar a conta de usuário** ? Se você desativar uma conta de usuário, pois um funcionário deixa, temporária ou permanentemente, a conta do usuário online também é desativada. A conta online não pode ser usada, mas os dados do usuário, incluindo email, são mantidos nos serviços Online da Microsoft. Para obter instruções, consulte [desativar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6) em [gerenciar contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md).

-   **Remover a conta de usuário** ? Se você remover uma conta de usuário, a conta online também será removida do Microsoft Online Services. Para obter instruções, consulte [remover uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove) em [gerenciar contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md).

    > [!WARNING]
    >  Esteja ciente de que quando uma conta online for removida, os dados do usuário estarão sujeito a políticas de retenção de dados dos serviços Online da Microsoft. Se você precisar manter os dados de usuário da pessoa s depois que um funcionário sair, desative a conta de usuário em vez de removê-la.

####  <a name="to-unassign-an-online-account-from-a-user-account"></a><a name="BKMK_ToUnassignAnOnlineAccount"></a>Para cancelar a atribuição de uma conta online de uma conta de usuário

1.  No Painel, clique em **Usuários**.

2.  Clique com o botão direito do mouse na conta de usuário na lista e, em seguida, clique em **Cancelar atribuição de uma conta do Microsoft Online** (no Windows Server Essentials, clique em **cancelar a atribuição de uma conta do Office 365**).

3.  No prompt de confirmação, clique em **Sim**.

##  <a name="manage-email-addresses-for-exchange-online"></a><a name="BKMK_SECTION_ManageEmailAddresses"></a>Gerenciar endereços de email para o Exchange Online
 Ao adicionar endereços de email à conta do usuário online no Windows Server Essentials, você pode permitir que o usuário receba emails em vários endereços de email no Exchange Online.

###  <a name="to-add-additional-email-addresses-to-a-user-s-microsoft-online-account"></a><a name="BKMK_PROC_AddEmailAliases"></a>Para adicionar outros endereços de email a uma conta online do usuário s Microsoft

1.  No painel do Windows Server Essentials, clique em **usuários**.

2.  Clique com botão direito do mouse na lista de contas de usuário e clique em **Exibir as propriedades de conta**.

3.  Na guia **online da Microsoft** das propriedades da conta (ou na guia **Office 365** do Windows Server Essentials), clique em **Adicionar**.

4.  Digite o novo alias de email e, em seguida, selecione o domínio de email.

5.  Clique em **OK** duas vezes.

##  <a name="manage-distribution-groups-for-exchange-online-windows-server-essentials-only"></a><a name="BKMK_SECTION_ManageDistributionGroups"></a>Gerenciar grupos de distribuição para o Exchange Online (somente Windows Server Essentials)
 Depois de integrar o servidor do Windows Server Essentials com o Office 365, você pode criar e gerenciar grupos de distribuição para o Exchange Online no painel do Windows Server Essentials. Você faz isso na guia **grupos de distribuição** que é adicionada à página **usuários** . Você só verá essa guia se tiver uma assinatura Online do Exchange. Esse recurso não está disponível no Windows Server Essentials.

 Use os procedimentos a seguir para:

-   [Adicionar um grupo de distribuição](#BKMK_PROCEDURE_AddDistGroup)

-   [Alterar os membros de um grupo de distribuição](#BKMK_ChangeGroupMembers)

-   [Alterar as associações de grupo de distribuição de um usuário](#BKMK_EditUserMemberships)

-   [Remover um grupo de distribuição](#BKMK_RemoveDistributionGroup)

###  <a name="to-add-a-distribution-group"></a><a name="BKMK_PROCEDURE_AddDistGroup"></a>Para adicionar um grupo de distribuição

1.  No painel do Windows Server Essentials, clique em **usuários**e, em seguida, clique na guia **grupos de distribuição** .

2.  Em **Tarefas do Grupo de Distribuição**, clique em **Adicionar um grupo de distribuição**.

     O Assistente de Adição de um Novo Grupo de Distribuição é exibido.

3.  Na página **Adicionar um novo grupo de distribuição**, insira as seguintes informações e clique em **Avançar**:

    -   Digite um nome de grupo, descrição opcional e alias de email para o novo grupo de distribuição.

    -   Por padrão, o grupo de distribuição pode receber email de pessoas fora da sua organização. Se você não deseja permitir isso, desmarque essa opção.

4.  Na página **Adicionar membros do grupo**, use o botão **Adicionar** para adicionar contas de usuário ativas que têm uma conta online atribuída a eles, e outros grupos de distribuição, ao novo grupo de distribuição. Em seguida, clique em **Próximo**.

     O novo grupo de distribuição é criado no Exchange Online.

###  <a name="to-change-the-members-of-a-distribution-group"></a><a name="BKMK_ChangeGroupMembers"></a>Para alterar os membros de um grupo de distribuição

1.  No Painel, clique em **Usuários** e clique na guia **Grupos de distribuição**.

2.  Clique com o botão direito do mouse na lista e clique em **Alterar a associação do grupo**.

3.  Use os botões **Adicionar** e **Remover** para adicionar ou remover contas online ativas do grupo de distribuição. Em seguida, clique em **Avançar** para atualizar a associação ao grupo de distribuição no Exchange Online.

###  <a name="to-change-a-user-s-distribution-group-memberships"></a><a name="BKMK_EditUserMemberships"></a>Para alterar as associações de grupo de distribuição de um usuário

1.  No Painel, clique em **Usuários**.

2.  Clique com botão direito do mouse na lista de contas de usuário e clique em **Exibir as propriedades de conta**.

3.  Nas propriedades da conta de usuário, clique na guia **Grupos de distribuição** e, em seguida, clique em **Editar**.

4.  Na caixa **Editar a associação a grupo**, use os botões **Adicionar** e **Remover** para adicionar ou remover grupos de distribuição da conta de usuário e, em seguida, clique em **Fechar**.

5.  Clique em **OK** para salvar as propriedades da conta de usuário atualizada.

###  <a name="to-remove-a-distribution-group"></a><a name="BKMK_RemoveDistributionGroup"></a>Para remover um grupo de distribuição

1.  No Painel, clique em **Usuários** e clique na guia **Grupos de distribuição**.

2.  Clique com o botão direito do mouse do grupo de distribuição da lista e clique em **Remover o grupo**.

3.  No prompt de confirmação, clique em **Excluir grupo**.

     O grupo de distribuição é removido do Exchange Online.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md)

-   [Gerenciar o Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)

-   [Gerenciar serviços online da Microsoft](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
