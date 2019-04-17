---
title: "Gerenciar contas Online para os usuários do Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c09f4cf6-4d12-49fe-9ae4-e6cb14027b9d
8author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 95f401bbec9bb503d19e2d9918a05851c04f7ef4
ms.sourcegitcommit: 877a50cd8d6e727048cdfac9b614a98ac3220876
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>Gerenciar contas Online para os usuários do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando você integrar seu servidor Windows Server Essentials com o Microsoft Office 365, você pode gerenciar suas contas online juntamente com contas de usuário do painel. Neste tópico, você ll Descubra o que você pode obter Gerenciando seus usuários de contas da Microsoft Online Services no painel, como criar e gerenciar contas online no painel e como gerenciar endereços de email e grupos de distribuição para o Exchange Online no painel.  

  
> [!NOTE]
>  Para gerenciar suas contas de serviços Online da Microsoft no Windows Server Essentials, você deve integrar seu servidor com o Office 365. Para obter instruções, consulte [gerenciar o Office 365](Manage-Office-365-in-Windows-Server-Essentials.md).  
  
> [!IMPORTANT]
>  Se você gerenciamento de contas online no Windows Server Essentials, você acostumados para ver as contas de serviços Online da Microsoft conhecidas como *contas do Office 365*. No painel do Windows Server Essentials, os rótulos foram alterados para *contas da Microsoft Online Services*, ou *contas online da Microsoft* para abreviar. As contas e os procedimentos são os mesmos; somente os rótulos alterados. A maioria dos procedimentos neste tópico usam o termo *conta online*.  
  
## <a name="in-this-topic"></a>Neste tópico  
  
-   [Por que devo gerenciar minhas contas online no painel?](#BKMK_WhyManageOnlineAccounts)  
  
-   [Criar contas online](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [Gerenciar contas online](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [Gerenciar endereços de email para o Exchange Online](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [Gerenciar grupos de distribuição para o Exchange Online](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a>Por que devo gerenciar minhas contas online no painel?  
 Quando você usar o painel para atribuir uma conta da Microsoft Online Services para uma conta de usuário, as senhas de conta são automaticamente sincronizadas, e você pode manter as duas contas juntos em todo o ciclo de vida de s de conta de usuário.  
  
 É conveniente para o usuário, que pode usar a mesma senha para acessar os recursos no servidor e no Office 365. E você pode aplicar os mesmos requisitos de senha para ter acesso aos recursos no Office 365 que exigem para seus recursos internos.  
  
### <a name="how-does-password-synchronization-work"></a>Como funciona a sincronização de senha?  
 Quando você usar o painel para atribuir uma conta da Microsoft Online Services para uma conta de usuário, a senha da conta de usuário é sincronizada automaticamente com a conta do usuário s online. Isso significa que um usuário só precisa de uma única senha para acessar os recursos no servidor e no Office 365. Além disso, você pode usar o mesmo nome para a conta de usuário e a ID online do usuário s.  
  
 Sincronização de senhas ocorre imediatamente e automaticamente quando um usuário muda a senha da sua conta de usuário de um computador ingressado no domínio ou usando o acesso via Web remoto.  
  
> [!IMPORTANT]
>  Se o Office 365 é integrado ao Windows Server Essentials, os usuários não devem alterar a senha da conta online da Microsoft do portal do Office 365. Isso interromperá a sincronização de senha.  
  
### <a name="simplified-account-creation"></a>Criação de conta simplificada  
 Há outra vantagem quando você cria suas contas online iniciais do painel. Você pode criar contas online para todos os seus usuários com uma única ação. Por outro lado, se seus funcionários estão usando o Office 365 já e você configurar um servidor Windows Server Essentials novo, você pode criar todas as suas contas de usuário das contas online com uma única ação. Para obter mais informações, consulte [criar contas online](#BKMK_SECTION_CreateOnlineAccounts).  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>Gerenciar endereços de email e grupos de distribuição do painel  
 Você poderá gerenciar seus endereços de email e grupos de distribuição para Exchange Online no painel. E você pode usar seu domínio da organização s Internet nos endereços de email. Você pode fazer tudo isso no painel, sem fazer logon no Office 365. (Você ll precisa estar usando o Windows Server Essentials para gerenciar grupos de distribuição do painel. Esse recurso não tem suporte no Windows Server Essentials.) Para obter mais informações, consulte [gerenciar endereços de email para o Exchange Online](#BKMK_SECTION_ManageEmailAddresses) e [gerenciar grupos de distribuição para o Exchange Online](#BKMK_SECTION_ManageDistributionGroups).  
  
### <a name="manage-the-user-account-and-online-account-together"></a>Gerenciar as contas de usuário e online juntos  
 E você pode gerenciar uma conta online junto com a conta de usuário em todo o ciclo de vida de s de conta. Se você desativar a conta de usuário, a conta online também é desativada no Microsoft Online Services. Se você remover uma conta de usuário, a conta online também é removida. Para obter mais informações, consulte [gerenciar contas online](#BKMK_SECTION_ManageOnlineAccounts).  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a>Criar contas online  
 Depois de integrar seu servidor com o Office 365, é vantagem para criar contas de serviços Online da Microsoft para os usuários do painel. Você ll têm uma grande flexibilidade na criação de contas online. Se você tiver uma nova assinatura do Office 365, você pode em massa-criar contas online para todos os seus usuários. Se você já tiver criado suas contas online no Office 365, não se preocupe. Se você configurar um novo servidor, você pode criar suas contas de usuário no servidor importando as contas online. E você pode atribuir um novo ou uma conta online existente quando você cria uma conta de usuário individual ou quando você adiciona uma conta online para uma conta de usuário existente.  
  
 **Requisitos de licença** será necessário uma licença de usuário para cada conta online que você criar. Verifique o **Office 365** página no painel para ver quantas licenças de usuário estão disponíveis por meio de sua assinatura do Office 365. Se você precisar adicionar mais licenças de usuário, você ll ser capaz de abrir sua assinatura do Office 365 no Office 365 para fazer isso.  
  
 **Acompanhamento para os usuários** depois de adicionar uma conta online para uma conta de usuário, o usuário é necessária para alterar a senha da sua conta de usuário na próxima vez que fizerem logon no. A nova senha é sincronizada imediatamente com sua conta online. Depois disso, eles podem usar a senha para efetuar login no Office 365 com sua ID online.  
  
> [!IMPORTANT]
>  Enfatize aos usuários que eles nunca devem alterar a senha da conta online no Office 365. A senha será alterada automaticamente sempre que eles alteram a senha de sua conta de usuário. Se ele alterar a senha online no Office 365, sincronização de senha será interrompida.  
  
 Use os procedimentos nesta seção para:  
  
-   [Crie em massa de contas online para suas contas de usuário existente](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [Importar contas de usuário de suas contas da Microsoft Online Services](#BKMK_ToImportUserAccounts)  
  
-   [Criar uma nova conta de usuário com uma conta online atribuída a ele](#BKMK_ToCreateaNewUserAccount)  
  
-   [Atribuir uma conta online para uma conta de usuário](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  Se estiver usando o Windows Server Essentials, você verá *conta do Office 365*, em vez de *conta da Microsoft Online Services* durante esses procedimentos. O processo é o mesmo, mas a terminologia mudou no Windows Server Essentials.  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a>Crie em massa de contas online para suas contas de usuário existente  
  
1.  Fazer logon no servidor como administrador e abra o painel do Windows Server Essentials.  
  
2.  No painel, abra o **usuários** página.  
  
3.  Em **usuários tarefas**, clique em **contas online da Microsoft adicionar**.  
  
     O **contas adicionar Microsoft Online Services** página do assistente exibe todas as contas de usuário que não têm uma conta online da Microsoft. Todas as contas estão selecionadas por padrão, e o nome do usuário é sugerido para o ID online da Microsoft. Se você tiver vinculado a um domínio da Internet personalizado à sua assinatura do Office 365, esse domínio será usado por padrão.  
  
4.  Sobre o **contas adicionar serviços Online da Microsoft** página, examine as contas que serão criadas. Por exemplo, procure os usuários que já tem uma conta online com uma ID online diferente e verifique se o domínio que você deseja usar em endereços de email está selecionado. Quando você termina de fazer qualquer necessárias alterações, clique em **próxima**.  
  
5.  Sobre o **licenças atribuir Microsoft Online Services** de página, selecionados serviços do Office 365, os usuários usarão. Quando você clica em **próxima**, começará a criação de conta.  
  
    > [!NOTE]
    >  Você deve ter uma licença de serviço no Office 365 para cada conta online. Você pode verificar suas licenças disponíveis e, se necessário, abra sua assinatura para adicionar licenças do **Office 365** página no painel.  
  
6.  Notifique os usuários que eles agora tem uma conta online da Microsoft. Eles devem alterar a senha de conta de usuário de rede antes de fazer logon Office 365. Para obter instruções, consulte [para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a>Para começar a usar uma nova conta online da Microsoft  
  
1.  Faça logon em seu computador com sua conta de usuário de rede.  
  
2.  Altere a senha de sua conta de usuário. Para começar, pressione Ctrl + Alt + Delete e clique em **alterar uma senha**.  
  
     Quando você alterar sua senha, a senha é sincronizada com sua nova conta online. Agora você pode usar a mesma senha para fazer logon no Office 365.  
  
3.  [Faça logon no Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) usando o novo ID online e a senha da sua conta de usuário.  
  
    > [!IMPORTANT]
    >  Não altere a senha de sua conta online no Office 365. Isso romperá sincronização de senhas. Sua senha online será atualizada sempre que você alterar a senha de sua conta de usuário de rede.  
  
###  <a name="BKMK_ToImportUserAccounts"></a>Para importar contas de usuário de suas contas online existentes  
  
1.  No painel, abra o **usuários** página.  
  
2.  Em **usuários tarefas**, clique em **importar contas do Office 365**.  
  
     A próxima página exibe todas as contas online para sua assinatura do Office 365 que não têm uma conta de usuário no servidor. Todas as contas estão selecionadas por padrão, e a ID online é sugerida para o nome de usuário.  
  
3.  Para criar as contas de usuário:  
  
    1.  Faça as alterações que são necessárias para as contas de usuário proposto.  
  
    2.  Opcionalmente, clique no link para exibir as senhas temporárias que serão atribuídas às contas de usuário. Você precisará dar a seus usuários a senha temporária juntamente com o nome da nova conta.  
  
         (Depois de criar as contas, você ll encontrar essas senhas listadas nesse arquivo: *SystemDrive*\Users\\*Office365admin*\\*NewServerUser*. txt onde *Office365admin* é a conta de rede que é usada para administrar o Office 365 no servidor e *NewServerUser* é o novo nome de conta de usuário.)  
  
    3.  Clique em **próxima** para criar as contas de usuário.  
  
4.  Avise seus usuários suas novas contas de usuário e as senhas temporárias usará para fazer logon servidor para o primeiro œ de tempo e que ele terá que alterar a senha depois que fizerem logon no. Para obter instruções para seus usuários, consulte [para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
     Certifique-se de que eles saibam que as senhas de suas contas online serão sincronizadas com sua conta de usuário futuramente, e eles não devem alterar suas senhas online no Office 365.  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a>Para criar uma nova conta de usuário com uma conta online atribuída a ele  
  
1.  No painel, clique em **usuários**.  
  
2.  Em **usuários tarefas**, clique em **adicionar uma conta de usuário**. Adicionar um Assistente de conta de usuário é exibida.  
  
3.  Siga as instruções para criar a conta de usuário.  
  
4.  Sobre o **atribuir uma conta da Microsoft Online Services** página, crie um novo online da conta do usuário ou atribuir uma conta online existente:  
  
    -   Para criar uma nova conta online, clique em **criar uma nova conta de serviços Online da Microsoft e o atribua a essa conta de usuário**e digite um nome para a conta da Microsoft Online Services (por padrão, o nome de usuário é usado para a ID online). Clique em **próxima**.  
  
    -   Para atribuir uma conta online da Microsoft existente, clique em **atribuir uma conta existente do Microsoft Online Services para essa conta de usuário**e selecione uma conta existente na lista suspensa. Clique em **próxima**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, contas de serviços Online da Microsoft são chamadas de contas do Office 365 em assistentes e rótulos de painel.  
  
5.  Siga as instruções para concluir o assistente.  
  
6.  Notifique o usuário precisará alterar a senha da conta de usuário antes de fazer logon Office 365 com a nova conta online. Para obter instruções, consulte [para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>Para atribuir uma conta online a uma conta de usuário  
  
1.  No painel, clique em **usuários**.  
  
2.  Clique com botão direito a conta de usuário na lista e clique em **atribuir uma conta online da Microsoft**. Atribuir um Assistente de conta de serviços Online da Microsoft é exibida.  
  
3.  Atribua uma conta online existente ou crie um novo para o usuário. A ID online padrão para uma nova conta é o nome de usuário. Clique em **próxima** para adicionar a conta online para a conta de usuário.  
  
4.  Examine as informações na última página do assistente e clique em **fechar**.  
  
5.  Notifique o usuário precisará alterar a senha da conta de usuário antes de fazer logon Office 365 com a nova conta online. Para obter instruções, consulte [para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a>Gerenciar contas online  
 Quando você adiciona uma conta online para uma conta de usuário no Windows Server Essentials, você pode gerenciar as duas contas juntos em todo o ciclo de vida de conta s.  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a>Noções básicas sobre o status de conta online  
 Quando você atribui uma conta da Microsoft Online Services para uma conta de usuário, o endereço de email da conta aparece no **conta online da Microsoft** coluna no **usuários** página do painel. (No Windows Server Essentials, o rótulo de coluna é **conta do Office 365**.)  
  
-   Um ícone azul ao lado de um endereço de email indica que a conta on-line está ativa. Ou seja, a conta tenha uma licença atual do Office 365 e o usuário pode usar a ID online para fazer logon no Office 365.  
  
-   Um ícone ao lado do endereço de email sombreado indica a conta on-line está inativo œ porque a licença não está mais ativa ou a conta online foi não atribuída. Quando você cancelar a atribuição de uma conta online do usuário s, a licença será removida e o usuário é impedido de fazer logon no Office 365 usando a conta. No entanto, o servidor mantém o mapeamento entre o nome da conta de usuário e o endereço de email do Office 365.  
  
###  <a name="BKMK_UnassignOnlineAccount"></a>Restringir o acesso a uma conta online  
 O que fazer se um usuário sair de sua organização, ou você deseja restringir o acesso do usuário s aos serviços do Office 365? Se você gerenciar suas contas de usuários online junto com suas contas de usuário no Windows Server Essentials, você terá três opções:  
  
-   **Cancelar a atribuição a conta online** ? Se você quiser impedir que um usuário usando o Office 365 sem impedir o acesso a recursos no servidor, você deve cancelar a atribuição a conta online. A licença do Office 365 será lançada, e o usuário é impedido de fazer logon no Office 365. No entanto, o servidor mantém o mapeamento entre o nome da conta de usuário e o endereço de email do Office 365. Para obter instruções, consulte [para cancelar a atribuição de uma conta online de uma conta de usuário](#BKMK_ToUnassignAnOnlineAccount).  
  
-   **Desativar a conta de usuário** ? Se você desativar uma conta de usuário porque um funcionário deixa, temporária ou permanentemente, a conta online do usuário s também é desativada. A conta online não pode ser usada, mas os dados do usuário, incluindo email, são mantidos no Microsoft Online Services. Para obter instruções, consulte [desativar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6) em [gerenciar contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
-   **Remover a conta de usuário** ? Se você remover uma conta de usuário, a conta on-line é removida da Microsoft Online Services também. Para obter instruções, consulte [remover uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove) em [gerenciar contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
    > [!WARNING]
    >  Lembre-se de que quando uma conta online é removida, os dados do usuário estão sujeito os dados políticas de retenção de serviços Online da Microsoft. Se você precisar manter os dados do usuário s pessoa depois que um funcionário deixa, desative a conta de usuário em vez de removê-lo.  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a>Para cancelar a atribuição de uma conta online de uma conta de usuário  
  
1.  No painel, clique em **usuários**.  
  
2.  Clique com botão direito a conta de usuário na lista e clique em **cancelar a atribuição de uma conta online da Microsoft** (no Windows Server Essentials, clique em **cancelar a atribuição de uma conta do Office 365**).  
  
3.  No prompt de confirmação, clique em **Sim**.  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a>Gerenciar endereços de email para o Exchange Online  
 Ao adicionar endereços de email para a conta de usuário s online no Windows Server Essentials, você pode permitir que o usuário receber email no vários endereços de email no Exchange Online.  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a>Para adicionar endereços de email adicionais para um usuário s conta online da Microsoft  
  
1.  No painel Windows Server Essentials, clique em **usuários**.  
  
2.  Clique com botão direito a conta de usuário na lista e clique em **exibir as propriedades da conta**.  
  
3.  Sobre o **Microsoft online** guia Propriedades da conta (ou o **Office 365** guia no Windows Server Essentials), clique em **adicionar**.  
  
4.  Digite o novo alias de email e, em seguida, selecione o domínio de email.  
  
5.  Clique em **OK** duas vezes.  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a>Gerenciar grupos de distribuição para o Exchange Online (somente Windows Server Essentials)  
 Depois de integrar seu servidor Windows Server Essentials com o Office 365, você pode criar e gerenciar grupos de distribuição para o Exchange Online no painel Windows Server Essentials. Você tudo isso no **grupos de distribuição** guia é adicionado ao **usuários** página. Você só verá este guia se você tiver uma assinatura do Exchange Online. Esse recurso não está disponível no Windows Server Essentials.  
  
 Use os procedimentos a seguir para:  
  
-   [Adicione um grupo de distribuição](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [Alterar os membros de um grupo de distribuição](#BKMK_ChangeGroupMembers)  
  
-   [Alterar um usuário s grupos de distribuição](#BKMK_EditUserMemberships)  
  
-   [Remover um grupo de distribuição](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a>Para adicionar um grupo de distribuição  
  
1.  No painel do Windows Server Essentials, clique em **usuários**e, em seguida, clique no **grupos de distribuição** guia.  
  
2.  Em **tarefas do grupo de distribuição**, clique em **adicionar um grupo de distribuição**.  
  
     Adicionar um novo Assistente de grupo de distribuição é exibida.  
  
3.  Sobre o **adicionar um novo grupo de distribuição** de página, insira as informações a seguir e clique em **próxima**:  
  
    -   Insira um nome do grupo, uma descrição opcional e um alias de email para o novo grupo de distribuição.  
  
    -   Por padrão, o grupo de distribuição pode receber emails das pessoas fora da sua organização. Se você não deseja permitir isso, desmarque essa opção.  
  
4.  Sobre o **adicionar membros do grupo** página, use o **adicionar** botão para adicionar contas de usuário ativa que tenham uma conta online atribuída a eles e distribuição de outra grupos, para o novo grupo de distribuição. Clique em **próxima**.  
  
     O novo grupo de distribuição é criado no Exchange Online.  
  
###  <a name="BKMK_ChangeGroupMembers"></a>Para alterar os membros de um grupo de distribuição  
  
1.  No painel, clique em **usuários**e, em seguida, clique no **grupos de distribuição** guia.  
  
2.  Clique com botão direito do grupo de distribuição na lista e clique em **alterar a associação ao grupo**.  
  
3.  Use o **adicionar** e **remover** botões para adicionar ou remover contas online ativas a partir do grupo de distribuição. Clique em **próxima** para atualizar a associação ao grupo de distribuição no Exchange Online.  
  
###  <a name="BKMK_EditUserMemberships"></a>Para alterar uma membros de grupos de distribuição de usuário s  
  
1.  No painel, clique em **usuários**.  
  
2.  Clique com botão direito a conta de usuário na lista e clique em **exibir as propriedades da conta**.  
  
3.  Nas propriedades da conta de usuário, clique no **grupos de distribuição** guia e, em seguida, clique em **editar**.  
  
4.  No **editar associação de grupo** caixa, use o **adicionar** e **remover** botões para adicionar ou remover grupos de distribuição da conta de usuário e, em seguida, clique em **fechar**.  
  
5.  Clique em **Okey** para evitar que o usuário atualizado propriedades da conta.  
  
###  <a name="BKMK_RemoveDistributionGroup"></a>Para remover um grupo de distribuição  
  
1.  No painel, clique em **usuários**e, em seguida, clique no **grupos de distribuição** guia.  
  
2.  Clique com botão direito do grupo de distribuição na lista e clique em **remover o grupo**.  
  
3.  No prompt de confirmação, clique em **excluir grupo**.  
  
     O grupo de distribuição é removido do Exchange Online.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [Gerencie o Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar Serviços Online da Microsoft](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
