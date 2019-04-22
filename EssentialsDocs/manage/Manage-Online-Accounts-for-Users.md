---
title: Gerenciar contas Online para usuários do Windows Server Essentials
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824087"
---
# <a name="manage-online-accounts-for-windows-server-essentials-users"></a>Gerenciar contas Online para usuários do Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Quando você integra seu servidor Windows Server Essentials com o Microsoft Office 365, você pode gerenciar suas contas online em conjunto com as contas de usuário do painel. Neste tópico, você encontrará o que você pode se beneficiar com gerenciamento de contas de serviços Online da Microsoft de usuários do painel, como criar e gerenciar contas online do painel e como gerenciar endereços de email e grupos de distribuição para o Exchange Online no painel.  

  
> [!NOTE]
>  Para gerenciar suas contas de serviços Online da Microsoft no Windows Server Essentials, você precisa integrar seu servidor com o Office 365. Para obter instruções, consulte [gerenciar o Office 365](Manage-Office-365-in-Windows-Server-Essentials.md).  
  
> [!IMPORTANT]
>  Se você gerenciar contas online no Windows Server Essentials, você se acostumar a ver contas da Microsoft Online Services, conhecidas como *contas do Office 365*. No painel do Windows Server Essentials, os rótulos foram alterados para *contas de serviços Online da Microsoft*, ou *contas online da Microsoft* de forma abreviada. As contas e os procedimentos são iguais, somente os rótulos foram alterados. A maioria dos procedimentos neste tópico usam o termo *conta online*.  
  
## <a name="in-this-topic"></a>Neste tópico  
  
-   [Por que devo gerenciar minhas contas online do painel?](#BKMK_WhyManageOnlineAccounts)  
  
-   [Criar contas online](#BKMK_SECTION_CreateOnlineAccounts)  
  
-   [Gerenciar contas online](#BKMK_SECTION_ManageOnlineAccounts)  
  
-   [Gerenciar endereços de email do Exchange Online](#BKMK_SECTION_ManageEmailAddresses)  
  
-   [Gerenciar grupos de distribuição para o Exchange Online](#BKMK_SECTION_ManageDistributionGroups)  
  
##  <a name="BKMK_WhyManageOnlineAccounts"></a> Por que devo gerenciar minhas contas online do painel?  
 Quando você usar o painel para atribuir uma conta de serviços Online da Microsoft para uma conta de usuário, as senhas da conta serão sincronizadas automaticamente, e você pode manter as duas contas juntas em todo o ciclo de vida de s de conta de usuário.  
  
 -S conveniente para o usuário, que pode usar a mesma senha para acessar recursos no servidor e no Office 365. E você pode aplicar os mesmos requisitos de senha para acesso aos recursos no Office 365 que você precisa de seus recursos internos.  
  
### <a name="how-does-password-synchronization-work"></a>Como funciona a sincronização de senha?  
 Quando você usar o painel para atribuir uma conta de serviços Online da Microsoft para uma conta de usuário, a senha da conta de usuário é sincronizada automaticamente com a conta do usuário s online. Isso significa que um usuário precisa apenas uma única senha para acessar os recursos no servidor e no Office 365. Além disso, você pode usar o mesmo nome para a conta de usuário e a ID de usuário s on-line.  
  
 A sincronização de senha ocorre imediatamente e automaticamente quando um usuário altera a senha da conta de usuário de um computador associado ao domínio ou usando o Acesso Remoto via Web.  
  
> [!IMPORTANT]
>  Se o Office 365 é integrado com o Windows Server Essentials, os usuários não devem alterar a senha para sua conta online da Microsoft no portal do Office 365. Isso interromperá a sincronização de senha.  
  
### <a name="simplified-account-creation"></a>Criação de contas simplificada  
 Daí s outra vantagem quando você cria suas contas online iniciais do painel. Você pode criar contas online para todos os usuários com uma única ação. Por outro lado, se seus funcionários já estiver usando Office 365 e você configurar um novo servidor do Windows Server Essentials, você pode criar todas as suas contas de usuário das contas online com uma única ação. Para obter mais informações, consulte [Criar contas online](#BKMK_SECTION_CreateOnlineAccounts).  
  
### <a name="manage-email-addresses-and-distribution-groups-from-the-dashboard"></a>Gerenciar endereços de email e grupos de distribuição do Painel  
 Você poderá gerenciar seus endereços de email e grupos de distribuição do Exchange Online do Painel. E você pode usar seu domínio da organização s Internet em endereços de email. Você pode fazer tudo isso do painel, sem entrar no Office 365. (Você precisará estar usando o Windows Server Essentials para gerenciar grupos de distribuição do painel. Esse recurso não é suportado no Windows Server Essentials.) Para obter mais informações, consulte [Gerenciar endereços de email para o Exchange Online](#BKMK_SECTION_ManageEmailAddresses) e [Gerenciar grupos de distribuição para o Exchange Online](#BKMK_SECTION_ManageDistributionGroups).  
  
### <a name="manage-the-user-account-and-online-account-together"></a>Gerenciar contas de usuário e online em conjunto  
 E você pode gerenciar uma conta online com a conta de usuário em todo o ciclo de vida de s de conta. Se você desativar a conta de usuário, a conta online também será desativada no Microsoft Online Services. Se você remover uma conta de usuário, a conta online também será removida. Para obter mais informações, consulte [Gerenciar contas online](#BKMK_SECTION_ManageOnlineAccounts).  
  
##  <a name="BKMK_SECTION_CreateOnlineAccounts"></a> Criar contas online  
 Após você integrar seu servidor com o Office 365, ele s a seu favor para Serviços Online da Microsoft de criar contas para os usuários do painel. Você terá uma grande flexibilidade na criação de contas online. Se você tiver uma nova assinatura do Office 365, você pode criar em massa contas online para todos os seus usuários. Se você já tiver criado suas contas online no Office 365, não se preocupe de t. Se você configurar um novo servidor, você pode criar suas contas de usuário no servidor importando as contas online. E você pode atribuir uma conta online existente ou um novo, quando você cria uma conta de usuário individual ou quando você adiciona uma conta online a uma conta de usuário.  
  
 **Requisitos de licença** você precisará de uma licença de usuário para cada conta online que você cria. Verifique as **Office 365** página no painel para ver quantas licenças de usuário estão disponíveis por meio de sua assinatura do Office 365. Se você precisar adicionar mais licenças de usuário, ll você conseguir abrir sua assinatura do Office 365 no Office 365 para fazer isso.  
  
 **Acompanhamento de usuários** depois de adicionar uma conta online para uma conta de usuário, o usuário é necessária para alterar a senha para sua conta de usuário na próxima vez que entrar. A nova senha é sincronizada imediatamente com sua nova conta online. Depois disso, é possível usar a senha para entrar no Office 365 com a ID online.  
  
> [!IMPORTANT]
>  Enfatize para os usuários que eles nunca devem alterar a senha da conta online no Office 365. A senha será alterada automaticamente sempre que eles alterarem a senha da conta de usuário. Se eles alterarem a senha online no Office 365, sincronização de senha será corrompida.  
  
 Use os procedimentos nesta seção para:  
  
-   [Criar contas online para suas contas de usuário existentes em massa](#BKMK_ToBulkCreateOnlineAccounts)  
  
-   [Importar contas de usuário das suas contas de serviços Online da Microsoft](#BKMK_ToImportUserAccounts)  
  
-   [Criar uma nova conta de usuário com uma conta online atribuída a ele](#BKMK_ToCreateaNewUserAccount)  
  
-   [Atribuir uma conta online a uma conta de usuário](#BKMK_ToAssignAnOnlineAccount)  
  
> [!NOTE]
>  Se estiver usando o Windows Server Essentials, você verá *conta do Office 365* em vez de *conta de serviços Online da Microsoft* no decorrer desses procedimentos. O processo é o mesmo, mas a terminologia mudou no Windows Server Essentials.  
  
###  <a name="BKMK_ToBulkCreateOnlineAccounts"></a> Para criar contas online para suas contas de usuário existentes em massa  
  
1.  Entrar no servidor como administrador e abra o painel do Windows Server Essentials.  
  
2.  No Painel, abra a página **Usuários**.  
  
3.  Em **Tarefas de usuários**, clique em **Adicionar contas online da Microsoft**.  
  
     A página **Adicionar contas dos serviços Online da Microsoft** do assistente exibe todas as contas de usuário que não têm uma conta online da Microsoft. Todas as contas são selecionadas por padrão e o nome de usuário é sugerido para a ID online da Microsoft. Se você tiver vinculado um domínio de Internet personalizado à sua assinatura do Office 365, esse domínio será usado por padrão.  
  
4.  Na página **Adicionar contas dos serviços Online da Microsoft** , examine as contas que serão criadas. Por exemplo, verifique os usuários que já tem uma conta online com uma ID online diferente e verifique se o domínio que você deseja usar em endereços de email está selecionado. Quando terminar de fazer quaisquer alterações necessárias, clique em **Avançar**.  
  
5.  Sobre o **licenças atribuir serviços Online da Microsoft** página, selecione seus usuários usarão os serviços do Office 365. Quando você clicar em **Avançar**, começará a criação de contas.  
  
    > [!NOTE]
    >  Você deve ter uma licença de serviço no Office 365 para cada conta online. Você pode verificar as licenças disponíveis e, se necessário, abrir sua assinatura para adicionar licenças na página **Office 365** no Painel.  
  
6.  Notifique os usuários de que agora eles têm uma conta online da Microsoft. Eles devem alterar suas senhas de conta de usuário de rede para que poderem entrar Office 365. Para obter instruções, consulte [Para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
###  <a name="BKMK_ToBeginUsingAnOnlineAccount"></a> Para começar a usar uma nova conta online da Microsoft  
  
1.  Entre em seu computador com a conta de usuário de rede.  
  
2.  Altere a senha da conta do usuário. Para começar, pressione Ctrl + Alt + Delete e clique em **Alterar uma senha**.  
  
     Quando você alterar sua senha, a senha será sincronizada com sua nova conta online. Agora você pode usar a mesma senha para entrar no Office 365.  
  
3.  [Entre no Office 365](https://login.microsoftonline.com/login.srf?wa=wsignin1.0&rpsnv=3&ct=1398981834&rver=6.1.6206.0&wp=MBI_SSL&wreply=https:%2F%2Foutlook.office365.com%2Fowa%2F&id=260563&CBCXT=out) usando sua nova ID online e sua senha de conta do usuário.  
  
    > [!IMPORTANT]
    >  Não altere sua senha da conta online no Office 365. Isso interromperá a sincronização de senha. Sua senha online será atualizada sempre que você alterar a senha para a conta de usuário de rede.  
  
###  <a name="BKMK_ToImportUserAccounts"></a> Para importar contas de usuário de suas contas online existentes  
  
1.  No Painel, abra a página **Usuários**.  
  
2.  No **Tarefas de usuários**, clique em **Importar contas do Office 365**.  
  
     A página seguinte exibe todas as contas online para sua assinatura do Office 365 que não têm uma conta de usuário no servidor. Todas as contas são selecionadas por padrão e a ID online é sugerida como nome de usuário.  
  
3.  Para criar as contas de usuário:  
  
    1.  Faça as alterações que são necessários para as contas de usuário propostas.  
  
    2.  Opcionalmente, clique no link para exibir as senhas temporárias que serão atribuídas às contas de usuário. Você precisará fornecer a seus usuários senhas temporárias juntamente com seu novo nome de conta.  
  
         (Depois de criar as contas, você encontrará essas senhas listadas neste arquivo: *SystemDrive*\Users\\*Office365admin*\\*NewServerUser*. txt em que *Office365admin* é a conta de rede que é usado para administrar o servidor do Office 365 e *NewServerUser* é o novo nome de conta de usuário.)  
  
    3.  Clique em **Avançar** para criar as contas de usuário.  
  
4.  Informe aos usuários as novas contas de usuário e as senhas temporárias que eles usarão para entrar no servidor para o primeiro œ tempo e que eles terão de alterar a senha depois de entrarem. Para obter instruções para seus usuários, consulte [para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
     Certifique-se de que eles saibam que as senhas de suas contas online serão sincronizadas com sua conta de usuário no futuro e que não devem alterar sua senha online no Office 365.  
  
###  <a name="BKMK_ToCreateaNewUserAccount"></a> Para criar uma nova conta de usuário com uma conta online atribuída a ele  
  
1.  No Painel, clique em **Usuários**.  
  
2.  Em **Tarefas de usuários**, clique em **Adicionar uma conta de usuário**. O Assistente de Adição de Conta de Usuário é exibido.  
  
3.  Siga as instruções para criar a conta de usuário.  
  
4.  Na página **Atribuir uma conta de serviços Online da Microsoft**, crie uma nova conta online para o usuário ou atribua uma conta online existente:  
  
    -   Para criar uma nova conta online, clique em **Criar uma nova conta de serviços Online da Microsoft e atribuí-la a essa conta de usuário**, e digite um nome para a conta de serviços Online da Microsoft (por padrão, o nome de usuário é usado para a ID online). Em seguida, clique em **Avançar**.  
  
    -   Para atribuir uma conta online ao Microsoft existente, clique em **Atribuir uma conta de serviços Online da Microsoft a esta conta de usuário**e selecione uma conta existente na lista suspensa. Em seguida, clique em **Avançar**.  
  
    > [!NOTE]
    >  No Windows Server Essentials, contas de serviços Online da Microsoft são chamadas de contas do Office 365 nos assistentes e rótulos do painel.  
  
5.  Siga as instruções para concluir o assistente.  
  
6.  Notifique o usuário de que ele deverá alterar a senha da conta de usuário antes de poder entrar no Office 365 com a nova conta online. Para obter instruções, consulte [Para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
#### <a name="BKMK_ToAssignAnOnlineAccount"></a>Para atribuir uma conta online a uma conta de usuário  
  
1.  No Painel, clique em **Usuários**.  
  
2.  Clique com botão direito na lista de contas de usuário e clique em **Atribuir uma conta da Microsoft online**. O Assistente de atribuição de uma conta de serviços Online da Microsoft é exibido.  
  
3.  Atribua uma conta online existente ou crie um novo usuário. A ID online padrão para uma nova conta é o nome de usuário. Em seguida, clique em **Avançar** para adicionar a conta online para a conta de usuário.  
  
4.  Examine as informações na última página do assistente e clique em **Fechar**.  
  
5.  Notifique o usuário de que ele deverá alterar a senha da conta de usuário antes de poder entrar no Office 365 com a nova conta online. Para obter instruções, consulte [Para começar a usar uma nova conta online da Microsoft](#BKMK_ToBeginUsingAnOnlineAccount).  
  
##  <a name="BKMK_SECTION_ManageOnlineAccounts"></a> Gerenciar contas online  
 Quando você adiciona uma conta online a uma conta de usuário no Windows Server Essentials, você pode gerenciar ambas as contas juntas em todo o ciclo de vida de conta s.  
  
###  <a name="BKMK_UnderstandingAccountStatus"></a> Noções básicas sobre o status da conta online  
 Quando você atribuir uma conta de serviços Online da Microsoft a uma conta de usuário, o endereço de email para a conta aparece na coluna **Conta online da Microsoft** na página **Usuários** do Painel. (No Windows Server Essentials, o rótulo da coluna é **conta do Office 365**.)  
  
-   Um ícone azul ao lado de um endereço de email indica que a conta online está ativa. Ou seja, a conta tem uma licença do Office 365 atual, e o usuário pode usar a ID online para entrar no Office 365.  
  
-   Um ícone sombreado ao lado do endereço de email indica a conta online está inativa œ porque a licença não está mais ativa ou a conta online foi desfeita. Quando você cancelar a atribuição de uma conta do usuário s online, a licença é removida e o usuário é impedido de entrar no Office 365 usando a conta. No entanto, o servidor mantém o mapeamento entre o nome da conta de usuário e o endereço de email do Office 365.  
  
###  <a name="BKMK_UnassignOnlineAccount"></a> Restringir o acesso a uma conta online  
 O que fazer se um usuário sair de sua organização, ou se você deseja restringir o acesso do usuário s para seus serviços do Office 365? Se você gerenciar suas contas de usuários online, juntamente com suas contas de usuário no Windows Server Essentials, você tem três opções:  
  
-   **Cancelar a atribuição da conta online** ? Se você quiser impedir que um usuário usando o Office 365 sem evitar o acesso aos recursos no servidor, você deve cancelar a atribuição da conta online. A licença do Office 365 será liberada e o usuário é impedido de entrar no Office 365. No entanto, o servidor mantém o mapeamento entre o nome da conta de usuário e o endereço de email do Office 365. Para obter instruções, consulte [para cancelar a atribuição de uma conta online de uma conta de usuário](#BKMK_ToUnassignAnOnlineAccount).  
  
-   **Desativar a conta de usuário** ? Se você desativar uma conta de usuário porque um funcionário saiu, temporária ou permanentemente, a conta do usuário s online também será desativada. A conta online não pode ser usada, mas os dados do usuário, incluindo email, são mantidos nos serviços Online da Microsoft. Para obter instruções, consulte [desativar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6) na [Manage User Accounts](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
-   **Remover a conta de usuário** ? Se você remover uma conta de usuário, a conta online será removida também dos Serviços Online da Microsoft. Para obter instruções, consulte [remover uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove) na [Manage User Accounts](Manage-User-Accounts-in-Windows-Server-Essentials.md).  
  
    > [!WARNING]
    >  Esteja ciente de que quando uma conta online for removida, os dados do usuário estarão sujeito a políticas de retenção de dados dos serviços Online da Microsoft. Se você precisar manter os dados do usuário s pessoa depois que um funcionário sair, desative a conta de usuário em vez de removê-lo.  
  
####  <a name="BKMK_ToUnassignAnOnlineAccount"></a> Para desatribuir uma conta online de uma conta de usuário  
  
1.  No Painel, clique em **Usuários**.  
  
2.  Clique com botão direito na lista de contas de usuário e, em seguida, clique em **cancelar a atribuição de uma conta online da Microsoft** (no Windows Server Essentials, clique em **cancelar a atribuição de uma conta do Office 365**).  
  
3.  No prompt de confirmação, clique em **Sim**.  
  
##  <a name="BKMK_SECTION_ManageEmailAddresses"></a> Gerenciar endereços de email do Exchange Online  
 Ao adicionar endereços de email para a conta de usuário s on-line no Windows Server Essentials, você pode permitir que o usuário receba emails em vários endereços de email no Exchange Online.  
  
###  <a name="BKMK_PROC_AddEmailAliases"></a> Para adicionar endereços de email adicionais a um usuário s conta online da Microsoft  
  
1.  No painel do Windows Server Essentials, clique em **usuários**.  
  
2.  Clique com botão direito do mouse na lista de contas de usuário e clique em **Exibir as propriedades de conta**.  
  
3.  Sobre o **online da Microsoft** guia Propriedades da conta (ou o **Office 365** guia no Windows Server Essentials), clique em **adicionar**.  
  
4.  Digite o novo alias de email e, em seguida, selecione o domínio de email.  
  
5.  Clique em **OK** duas vezes.  
  
##  <a name="BKMK_SECTION_ManageDistributionGroups"></a> Gerenciar grupos de distribuição para o Exchange Online (Windows Server Essentials apenas)  
 Após você integrar seu servidor Windows Server Essentials com o Office 365, você pode criar e gerenciar grupos de distribuição do Exchange Online do painel do Windows Server Essentials. Ll você fazer isso na **grupos de distribuição** guia é adicionado para o **usuários** página. Você só verá essa guia se tiver uma assinatura Online do Exchange. Esse recurso não está disponível no Windows Server Essentials.  
  
 Use os procedimentos a seguir para:  
  
-   [Adicionar um grupo de distribuição](#BKMK_PROCEDURE_AddDistGroup)  
  
-   [Alterar os membros de um grupo de distribuição](#BKMK_ChangeGroupMembers)  
  
-   [Alterar associações de grupo de distribuição um usuário s](#BKMK_EditUserMemberships)  
  
-   [Remover um grupo de distribuição](#BKMK_RemoveDistributionGroup)  
  
###  <a name="BKMK_PROCEDURE_AddDistGroup"></a> Para adicionar um grupo de distribuição  
  
1.  No painel do Windows Server Essentials, clique **os usuários**e, em seguida, clique no **grupos de distribuição** guia.  
  
2.  Em **Tarefas do Grupo de Distribuição**, clique em **Adicionar um grupo de distribuição**.  
  
     O Assistente de Adição de um Novo Grupo de Distribuição é exibido.  
  
3.  Na página **Adicionar um novo grupo de distribuição**, insira as seguintes informações e clique em **Avançar**:  
  
    -   Digite um nome de grupo, descrição opcional e alias de email para o novo grupo de distribuição.  
  
    -   Por padrão, o grupo de distribuição pode receber email de pessoas fora da sua organização. Se você não deseja permitir isso, desmarque essa opção.  
  
4.  Na página **Adicionar membros do grupo**, use o botão **Adicionar** para adicionar contas de usuário ativas que têm uma conta online atribuída a eles, e outros grupos de distribuição, ao novo grupo de distribuição. Em seguida, clique em **Avançar**.  
  
     O novo grupo de distribuição é criado no Exchange Online.  
  
###  <a name="BKMK_ChangeGroupMembers"></a> Para alterar os membros de um grupo de distribuição  
  
1.  No Painel, clique em **Usuários** e clique na guia **Grupos de distribuição**.  
  
2.  Clique com o botão direito do mouse na lista e clique em **Alterar a associação do grupo**.  
  
3.  Use os botões **Adicionar** e **Remover** para adicionar ou remover contas online ativas do grupo de distribuição. Em seguida, clique em **Avançar** para atualizar a associação ao grupo de distribuição no Exchange Online.  
  
###  <a name="BKMK_EditUserMemberships"></a> Para alterar associações de grupo de distribuição um usuário s  
  
1.  No Painel, clique em **Usuários**.  
  
2.  Clique com botão direito do mouse na lista de contas de usuário e clique em **Exibir as propriedades de conta**.  
  
3.  Nas propriedades da conta de usuário, clique na guia **Grupos de distribuição** e, em seguida, clique em **Editar**.  
  
4.  Na caixa **Editar a associação a grupo** , use os botões **Adicionar** e **Remover** para adicionar ou remover grupos de distribuição da conta de usuário e, em seguida, clique em **Fechar**.  
  
5.  Clique em **OK** para salvar as propriedades da conta de usuário atualizada.  
  
###  <a name="BKMK_RemoveDistributionGroup"></a> Para remover um grupo de distribuição  
  
1.  No Painel, clique em **Usuários** e clique na guia **Grupos de distribuição**.  
  
2.  Clique com o botão direito do mouse do grupo de distribuição da lista e clique em **Remover o grupo**.  
  
3.  No prompt de confirmação, clique em **Excluir grupo**.  
  
     O grupo de distribuição é removido do Exchange Online.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Office 365](Manage-Office-365-in-Windows-Server-Essentials.md)  
  
-   [Gerenciar Serviços Online da Microsoft](Manage-Microsoft-Online-Services-in-Windows-Server-Essentials.md)
