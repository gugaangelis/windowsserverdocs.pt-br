---
title: "Gerenciar contas de usuário no Windows Server Essentials"
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d115697-532b-48c2-a659-9f889e235326
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 91175836e4453860b17d2655e6a5a831645de410
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>Gerenciar contas de usuário no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

A página de usuários do Windows Server Essentials painel centraliza informações e tarefas que ajudarão a gerenciem as contas de usuário em sua rede de pequena empresa. Para obter uma visão geral do painel de usuários, consulte [visão geral do painel](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).  
  
  
##  <a name="BKMK_ManageAccounts"></a>Gerenciar contas de usuário  
 Os tópicos a seguir fornecem informações sobre como usar o Windows Server Essentials Dashboard para gerenciar as contas de usuário no servidor:  
  
-   [Adicionar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)  
  
-   [Remover uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)  
  
-   [Contas de usuário de modo de exibição](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)  
  
-   [Alterar o nome de exibição da conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)  
  
-   [Ativar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)  
  
-   [Desativar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)  
  
-   [Entender as contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)  
  
-   [Gerenciar contas de usuário usando o painel](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)  
  
###  <a name="BKMK_Manage1"></a>Adicionar uma conta de usuário  
 Quando você adiciona uma conta de usuário, o usuário atribuído pode fazer logon na rede, e você pode dar ao usuário permissão para acessar recursos de rede, como pastas compartilhadas e o site de acesso via Web remoto. Windows Server Essentials inclui um Assistente de conta de usuário que ajuda você a adicionar:  
  
-   Forneça um nome e senha da conta de usuário.  
  
-   Defina a conta como um administrador ou como um usuário padrão.  
  
-   Selecione pastas que pode acessar a conta de usuário que compartilhadas.  
  
-   Especifique se a conta de usuário tem acesso remoto à rede.  
  
-   Selecione opções de email, se aplicável.  
  
-   Atribua uma conta da Microsoft Online Services (chamada de uma conta do Office 365 no Windows Server Essentials), se aplicável.  
  
-   Atribua grupos de usuários (somente Windows Server Essentials).  
  
> [!NOTE]
>  -   Caracteres não ASCII não têm suporte no Microsoft Azure Active Directory (Azure AD). Não use quaisquer caracteres não ASCII em sua senha, se o servidor está integrado com o Azure AD.  
> -   As opções de email estão disponíveis somente se você instalar um suplemento que fornece o serviço de email.  
  
##### <a name="to-add-a-user-account"></a>Para adicionar uma conta de usuário  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  No **usuários tarefas** painel, clique em **adicionar uma conta de usuário**. Adicionar um Assistente de conta de usuário é exibida.  
  
4.  Siga as instruções para concluir o assistente.  
  
###  <a name="BKMK_Remove"></a>Remover uma conta de usuário  
 Quando você optar por remover uma conta de usuário do servidor, um assistente exclui a conta selecionada. Por isso, você pode usar não é mais a conta para fazer logon na rede ou acessar qualquer um dos recursos da rede. Como opção, você também pode excluir os arquivos para a conta de usuário ao mesmo tempo que você remova a conta. Se você não quiser remover permanentemente a conta de usuário, você pode desativar a conta de usuário em vez disso, para suspender o acesso aos recursos de rede.  
  
> [!IMPORTANT]
>  Se uma conta de usuário tem um Microsoft online conta atribuído, quando você remover a conta de usuário, a conta online também é removida do Microsoft Online Services e os dados do usuário s, incluindo email, estão sujeito às dados políticas de retenção no Microsoft Online Services. Se você quiser manter os dados do usuário para a conta online, desative a conta de usuário em vez de removê-lo. Para obter mais informações, consulte [gerenciar contas Online para que os usuários](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account"></a>Para remover uma conta de usuário  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja remover.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **remover a conta de usuário**. Excluir um Assistente de conta de usuário é exibida.  
  
5.  Sobre o **você deseja manter os arquivos? **página do assistente, você pode optar por excluir os arquivos de usuário s, inclusive backups de histórico de arquivos e a pasta redirecionada para a conta de usuário. Para manter o usuário arquivos s, deixe a caixa de seleção vazia. Depois de fazer sua seleção, clique em **próxima**.  
  
6.  Clique em **excluir conta**.  
  
> [!NOTE]
>  Depois de remover uma conta de usuário, a conta não aparece na lista de contas de usuário. Se você optar por excluir os arquivos, o servidor exclui permanentemente a pasta do usuário s do **usuários** pasta de servidor e do **Backups de histórico de arquivos** pasta do servidor.  
>   
>  Se você tiver um provedor de email integrado, a conta de email atribuída à conta de usuário também será removida.  
  
###  <a name="BKMK_Manage3"></a>Contas de usuário de modo de exibição  
 O **usuários** seção do Windows Server Essentials painel exibe uma lista de contas de usuário de rede. A lista também fornece informações adicionais sobre cada conta.  
  
##### <a name="to-view-a-list-of-user-accounts"></a>Para exibir uma lista de contas de usuário  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação principal, clique em **usuários**.  
  
3.  O painel exibe uma lista de contas de usuário atual.  
  
##### <a name="to-view-or-change-properties-for-a-user-account"></a>Para exibir ou alterar as propriedades de uma conta de usuário  
  
1.  Na lista de contas de usuário, selecione a conta para o qual você deseja exibir ou alterar propriedades.  
  
2.  No **< usuário Account\ > tarefas** painel, clique em **exibir as propriedades da conta**. O **propriedades** página para a conta de usuário é exibido.  
  
3.  Clique em uma guia para exibir as propriedades para esse recurso de conta.  
  
4.  Para salvar as alterações feitas nas propriedades da conta de usuário, clique em **aplicar**.  
  
###  <a name="BKMK_Manage4"></a>Alterar o nome de exibição da conta de usuário  
 O nome de exibição é o nome que aparece no **nome** coluna no **usuários** página do painel. Alterar o nome de exibição não altera o nome de logon ou entrada de uma conta de usuário.  
  
##### <a name="to-change-the-display-name-for-a-user-account"></a>Para alterar o nome de exibição para uma conta de usuário  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja alterar.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **exibir as propriedades da conta**. O **propriedades** página para a conta de usuário é exibido.  
  
5.  No **geral** guia, digite um novo **nome** e **Sobrenome** para a conta de usuário e clique em **Okey**.  
  
     O novo nome de exibição aparece na lista de contas de usuário.  
  
###  <a name="BKMK_Manage5"></a>Ativar uma conta de usuário  
 Quando você ativa uma conta de usuário, o usuário atribuído pode fazer logon para os recursos de rede de rede e acesso aos quais a conta tem permissão, como pastas compartilhadas e o site de acesso via Web remoto.  
  
> [!NOTE]
>  Você pode ativar apenas uma conta de usuário que está desativada. Você não pode ativar uma conta de usuário após a remoção do servidor.  
  
##### <a name="to-activate-a-user-account"></a>Para ativar uma conta de usuário  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na exibição de lista, selecione a conta de usuário que você deseja ativar.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **ativar a conta de usuário**.  
  
5.  Na janela de confirmação, clique em **Sim** para confirmar a ação.  
  
> [!NOTE]
>  Depois de ativar uma conta de usuário, exibe o status da conta **Active**. A conta de usuário retoma os mesmos direitos de acesso que foram atribuídos antes da desativação da conta.  
>   
>  Se você tiver um provedor de email integrado, a conta de email atribuída à conta de usuário também será ativada.  
  
###  <a name="BKMK_Manage6"></a>Desativar uma conta de usuário  
 Quando você desativa uma conta de usuário, o acesso de contas para o servidor está temporariamente suspensa. Por isso, o usuário atribuído não pode usar a conta para acessar o site de acesso via Web remoto ou recursos de rede, como pastas compartilhadas até que você ativa a conta.  
  
 Se a conta de usuário tiver uma conta online da Microsoft atribuída, a conta online também é desativada. O usuário não pode usar recursos no Office 365 e outros serviços online que você assine, mas os dados do usuário s, incluindo email, são mantidos no Microsoft Online Services.  
  
> [!NOTE]
>  Você pode desativar somente uma conta de usuário que está atualmente ativa.  
  
##### <a name="to-deactivate-a-user-account"></a>Para desativar uma conta de usuário  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na exibição de lista, selecione a conta de usuário que você deseja desativar.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **desativar a conta de usuário**.  
  
5.  Na janela de confirmação, clique em **Sim** para confirmar a ação.  
  
> [!NOTE]
>  Depois que você desativa uma conta de usuário, exibe o status da conta **inativo**.  
>   
>  Se você tiver um provedor de email integrado, a conta de email atribuída à conta de usuário também será desativada.  
  
###  <a name="BKMK_Manage7"></a>Entender as contas de usuário  
 Uma conta de usuário fornece informações importantes para o Windows Server Essentials, que permite que pessoas acessem informações que são armazenadas no servidor e torna possível para os usuários individuais criar e gerenciar seus arquivos e configurações. Os usuários podem fazer logon em qualquer computador da rede se eles têm uma conta de usuário do Windows Server Essentials e têm permissões para acessar um computador. Os usuários acessem suas contas de usuário com seu nome de usuário e senha.  
  
 Há dois tipos principais de contas de usuário. Cada tipo oferece aos usuários um nível diferente de controle do computador:  
  
-   **Padrão** contas são para o dia a dia. A conta padrão ajuda a proteger sua rede, impedindo que os usuários façam alterações que afetam outros usuários, como excluir arquivos ou alterar configurações de rede.  
  
-   **Administrador** contas oferecem mais controle sobre uma rede do computador. Você deve atribuir o administrador da conta tipo somente quando necessário.  
  
###  <a name="BKMK_Manage8"></a>Gerenciar contas de usuário usando o painel  
 Windows Server Essentials torna possível realizar tarefas administrativas comuns usando o painel do Windows Server Essentials. Por padrão, o **usuários** página do painel inclui duas guias **usuários** e **grupos de usuários**.  
  
> [!NOTE]
>  -   Se integrar seu servidor que está executando o Windows Server Essentials com o Office 365, uma nova guia chamado **grupos de distribuição** também é adicionada dentro do **usuários** página do painel.  
> -   No Windows Server Essentials, o **usuários** página do painel inclui apenas uma única guia - **usuários**.  
  
 O **usuários** guia inclui o seguinte:  
  
-   Uma lista de contas de usuário, que exibe:  
  
    -   O nome do usuário.  
  
    -   O nome de Logon da conta de usuário.  
  
    -   Se a conta de usuário tem permissão de acesso em qualquer local. Em qualquer lugar permissão de acesso para uma conta de usuário é **permitidos** ou **não permitido**.  
  
    -   Se o histórico de arquivos para essa conta de usuário é gerenciado pelo servidor que executa o Windows Server Essentials. O status do histórico de arquivos para uma conta de usuário seja **gerenciado** ou **não gerenciado**.  
  
    -   O nível de acesso atribuído à conta de usuário. Você pode atribuir um **usuário padrão** acesso ou **administrador** acesso para uma conta de usuário.  
  
    -   O status de conta de usuário. Uma conta de usuário pode ser **Active**, **inativo**, ou **incompleto**.  
  
    -   No Windows Server Essentials, se o servidor está integrado com o Office 365 ou o Windows Intune, a conta online da Microsoft é exibida.  
  
    -   No Windows Server Essentials, se o servidor está integrado com o Microsoft Office 365, o status da conta do Office 365 (conhecida no Windows Server Essentials como a conta online da Microsoft) para a conta de usuário é exibido.  
  
-   Um painel de detalhes com informações adicionais sobre uma conta de usuário selecionado.  
  
-   Um painel de tarefas que inclui:  
  
    -   Um conjunto de tarefas administrativas de conta de usuário, como exibir e remover as contas de usuário e alterar as senhas.  
  
    -   Tarefas que permitem que você definir ou alterar as configurações de todas as contas de usuário na rede globalmente.  
  
 A tabela a seguir descreve as várias tarefas de conta de usuário que estão disponíveis no **usuários** guia. Algumas das tarefas são específicas de conta de usuário, e eles só ficam visíveis quando você seleciona uma conta de usuário na lista.  
  
> [!NOTE]
>  Se você integrar o Office 365 com o Windows Server Essentials, tarefas adicionais serão disponibilizados. Para obter mais informações, consulte [gerenciar contas Online para que os usuários](Manage-Online-Accounts-for-Users.md).  
  
### <a name="user-account-tasks-in-the-dashboard"></a>Tarefas de conta de usuário no painel  
  
|Nome da tarefa|Descrição|  
|---------------|-----------------|  
|Exibir as propriedades da conta|Permite que você exibir e alterar as propriedades da conta de usuário selecionado e para especificar as permissões de acesso de pasta para a conta.|  
|Desativar a conta de usuário|Uma conta de usuário que está desativada não pode fazer logon nas rede ou acessar recursos da rede, como impressoras ou pastas compartilhadas.|  
|Ativar a conta de usuário|Uma conta de usuário que é ativada pode fazer logon rede e pode acessar recursos de rede, conforme definido pelas permissões da conta.|  
|Remover a conta de usuário|Permite que você remova a conta de usuário selecionado.|  
|Alterar a senha da conta de usuário|Permite que você redefina a senha de rede para a conta de usuário selecionado.|  
|Adicionar uma conta de usuário|Inicia a adicionar um Assistente de conta de usuário, que permite que você crie uma nova conta de usuário único que tem acesso de usuário padrão ou acesso de administrador.|  
|Atribuir uma conta online da Microsoft|Adiciona uma conta online da Microsoft para a conta de usuário de rede local está selecionada.<br /><br /> Essa tarefa é exibida quando o servidor está integrado com os serviços online da Microsoft, como o Office 365.|  
|Adicionar contas online da Microsoft|Adiciona contas online da Microsoft e associa-los para contas de usuário de rede local.<br /><br /> Essa tarefa é exibida quando o servidor está integrado com os serviços online da Microsoft, como o Office 365.|  
|Defina a política de senha|Permite que você altere os valores da senha políticas com base em sua rede.|  
|Importar contas online da Microsoft|Executa uma importação em massa de contas de serviços online da Microsoft na rede local.<br /><br /> Essa tarefa é exibida quando o servidor está integrado com os serviços online da Microsoft, como o Office 365.|  
|Atualizar|Atualiza a guia de usuários.<br /><br /> Essa tarefa é aplicável ao Windows Server Essentials.|  
|Alterar as configurações do histórico de arquivos|Permite que você altere as configurações de histórico de arquivos, como backup frequência ou duração do backup.<br /><br /> Essa tarefa é aplicável ao Windows Server Essentials.|  
|Exportar todas as conexões remotas|Cria um. Arquivo de formato CSV de todas as conexões remotas com o servidor que ocorreram nos últimos 30 dias.|  
  
##  <a name="BKMK_ManageAccess"></a>Gerenciamento de senhas e acesso  
 Os tópicos a seguir fornecem informações sobre como usar o Windows Server Essentials Dashboard para gerenciar o acesso de usuário às pastas compartilhadas no servidor e senhas de conta de usuário:  
  
-   [Alterar ou redefinir a senha de uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)  
  
-   [O que você deve saber sobre políticas de senha](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)  
  
-   [Alterar a política de senha](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)  
  
-   [Nível de acesso a pastas compartilhadas](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)  
  
-   [Manter e gerenciar o acesso a arquivos para contas de usuário removido](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)  
  
-   [Sincronizar a senha DSRM com a senha de administrador de rede](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)  
  
-   [Dar permissão da área de trabalho remota a contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)  
  
-   [Permitir que os usuários acessem os recursos no servidor](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)  
  
-   [Alterar as permissões de acesso remoto para uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)  
  
-   [Alterar as permissões de rede virtual privada para uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)  
  
-   [Alterar o acesso a pastas compartilhadas internas para uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)  
  
-   [Permitir que as contas de usuário estabelecer uma sessão da área de trabalho remota em seus computadores](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)  
  
###  <a name="BKMK_Access1"></a>Alterar ou redefinir a senha de uma conta de usuário  
 Para alterar ou redefinir uma senha de conta de usuário, siga estas etapas.  
  
##### <a name="to-reset-the-password-for-a-user-account"></a>Para redefinir a senha de uma conta de usuário  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja redefinir.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **alterar a senha da conta de usuário**. O Assistente de senha de conta de usuário de alteração aparece.  
  
5.  Digite uma nova senha da conta de usuário e, em seguida, digite a senha novamente para confirmá-la.  
  
6.  Clique em **alterar senha**.  
  
7.  Fornece a nova senha para o usuário.  
  
    > [!IMPORTANT]
    >  -   Você não consiga alterar sua senha, se a política de senha para sua conta tiver sido definida **as senhas nunca expirem**.  
    > -   Caracteres não ASCII não são suportados no Azure AD. Portanto, se o servidor está integrado com o Azure AD, não use quaisquer caracteres não ASCII em sua senha.  
    > -   Se uma conta online da Microsoft (conhecida no Windows Server Essentials como uma conta do Office 365) é atribuída ao usuário, a senha será sincronizada com a senha da conta online. O usuário usará a nova senha para fazer logon no servidor ou fazer logon no Office 365. Para obter mais informações, consulte [gerenciar contas Online para que os usuários](Manage-Online-Accounts-for-Users.md).  
  
###  <a name="BKMK_Access3"></a>O que você deve saber sobre políticas de senha  
 A política de senha é um conjunto de regras que definem como os usuários criarem e usam senhas. A política ajuda a impedir o acesso não autorizado aos dados de usuário e outras informações que são armazenadas no servidor. A política de senha é aplicada a todas as contas de usuário que acessam a rede.  
  
 A política de senha do Windows Server Essentials consiste em três elementos principais da seguinte maneira:  
  
-   **Tamanho de senha**.  Quanto mais tempo uma senha é, mais seguro. Senhas em branco não são seguras.  
  
-   **Complexidade de senha**.  Senhas complexas contêm uma mistura de letras maiusculas e letras minúsculas (a-z, a Z), base (0-9) de números e símbolos não alfabéticos (como;!, @, #, _-). Senhas complexas são muito menos vulneráveis a acesso não autorizado. As senhas que contêm os nomes de usuário, datas de nascimento ou outras informações pessoais não fornecer segurança adequada.  
  
-   **Duração da senha**.  Windows Server Essentials requer que os usuários alterar a senha de pelo menos uma vez a cada 180 dias. Como opção, você pode optar por ter senhas nunca expirarão.  
  
 Para tornar mais fácil de implementar uma política de senha em sua rede do computador, o Windows Server Essentials fornece uma ferramenta simple que permite que você definir ou alterar a política de senha para qualquer um dos seguintes perfis predefinidos política quatro:  
  
-   **Fracas**.  Os usuários podem especificar qualquer senha que não está em branco.  
  
-   **Médio**.  Essas senhas devem conter pelo menos 5 caracteres. Uma senha complexa não é necessária.  
  
-   **Forte médio**.  Essas senhas devem conter pelo menos 5 caracteres e devem incluir letras, números e símbolos.  
  
-   **Forte**.  Essas senhas devem conter pelo menos 7 caracteres e devem incluir letras, números e símbolos. Essas senhas são mais seguras, mas podem ser mais difícil para os usuários se lembrarem.  
  
    > [!NOTE]
    >  As senhas não podem conter o usuário nome ou endereço de email.  
    >   
    >  Se você integrar com o Office 365, a integração impõe a **forte** política de senha e atualiza a política para incluir os seguintes requisitos:  
    >   
    >  -   As senhas devem conter 16 de 8 caracteres.  
    > -   Senhas não podem conter um espaço ou o nome de email do Office 365.  
  
 Por padrão, a instalação do servidor define a política de senha padrão o **forte** opção.  
  
###  <a name="BKMK_Access4"></a>Alterar a política de senha  
 Use o procedimento a seguir para definir ou alterar a política de senha para qualquer uma das quatro perfis predefinidos política.  
  
##### <a name="to-change-the-password-policy"></a>Para alterar a política de senha  
  
1.  Abrir o painel do Windows Server Essentials e clique em **usuários**.  
  
2.  No **usuários tarefas** painel, clique em **definir a política de senha**.  
  
3.  Sobre o **alterar a política de senha** tela, defina o nível de força de senha, movendo o controle deslizante.  
  
     A Microsoft recomenda que você definir a força de senha para **forte**.  
  
    > [!NOTE]
    >  Como opção, você também pode selecionar **as senhas nunca expirem**. Essa configuração é menos segura e, portanto ele não é recomendado.  
  
4.  Clique em **alterar política**.  
  
###  <a name="BKMK_Access5"></a>Nível de acesso a pastas compartilhadas  
 Como prática recomendada, você deve atribuir as permissões mais restritivas disponíveis que ainda permitem que os usuários realizem tarefas necessárias.  
  
 Você tem três configurações de acesso disponíveis para as pastas compartilhadas no servidor:  
  
-   **Leitura/gravação**.  Escolha esta configuração se você deseja permitir que a permissão de conta de usuário criar, alterar e excluir todos os arquivos na pasta compartilhada.  
  
-   **Somente leitura**.  Escolha essa configuração se você quiser conceder a permissão de conta de usuário para ler somente os arquivos na pasta compartilhada. Contas de usuário com acesso somente leitura não podem criar, alterar ou excluir todos os arquivos na pasta compartilhada.  
  
-   **Sem acesso**.  Escolha essa configuração se você não quiser que a conta de usuário para acessar todos os arquivos na pasta compartilhada.  
  
###  <a name="BKMK_Access6"></a>Manter e gerenciar o acesso a arquivos para contas de usuário removido  
 O administrador da rede pode remover uma conta de usuário e optar por manter o usuário arquivos s para uso futuro. Nesse cenário, a conta de usuário removido não pode mais ser usada para entrar em rede; No entanto, os arquivos para este usuário serão salvos em uma pasta compartilhada, que pode ser compartilhada com outro usuário.  
  
> [!IMPORTANT]
>  Lembre-se de que se você remover uma conta de usuário que tem uma conta online da Microsoft atribuída, a conta online também é removida, e os dados do usuário, incluindo email, estão sujeito às políticas de retenção de dados no Microsoft Online Services. Para manter os dados do usuário para a conta online, desative a conta de usuário em vez de removê-lo. Para obter mais informações, consulte [gerenciar contas Online para que os usuários](Manage-Online-Accounts-for-Users.md).  
  
##### <a name="to-remove-a-user-account-but-retain-access-to-the-user-s-files"></a>Para remover uma conta de usuário, mas manter o acesso aos arquivos do usuário s  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja remover.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **remover a conta de usuário**. Excluir um Assistente de conta de usuário é exibida.  
  
5.  Sobre o **você deseja manter os arquivos? ** de página, certifique-se de que o **excluir os arquivos, inclusive backups de histórico de arquivo e pasta redirecionada para essa conta de usuário** verificar caixa é clara e clique em **próxima**.  
  
     Uma página de confirmação é exibida avisando que está excluindo a conta, mas manter os arquivos.  
  
6.  Clique em **excluir conta** para remover a conta de usuário.  
  
 Depois que a conta de usuário for removida, o administrador pode dar acesso à outra conta de usuário para a pasta compartilhada.  
  
##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>Para dar uma permissão de conta de usuário para acessar uma pasta compartilhada  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **armazenamento**e, em seguida, clique no **pastas de servidor** guia.  
  
3.  Na lista de pastas, selecione o **usuários** pasta.  
  
4.  No **usuários tarefas** painel, clique em **abra a pasta**. Windows Explorer abre e exibe o conteúdo do **usuários** pasta.  
  
5.  Clique com botão direito na pasta para a conta de usuário que você deseja compartilhar e, em seguida, clique em **propriedades**.  
  
6.  Em **< usuário Account\ > propriedades**, clique no **compartilhamento** guia e, em seguida, clique em **compartilhamento**.  
  
7.  No **compartilhamento de arquivos** janela, digite ou selecione o nome da conta de usuário com quem deseja compartilhá-la e, em seguida, clique em **adicionar**.  
  
8.  Escolha o **nível de permissão** que você deseja que a conta de usuário e, em seguida, clique em **compartilhamento**.  
  
###  <a name="BKMK_Access7"></a>Sincronizar a senha DSRM com a senha de administrador de rede  
 Modo de restauração de serviços de diretório (DSRM) é um modo especial para inicialização para reparar ou recuperar do Active Directory. O sistema operacional usa DSRM para fazer logon no computador, se Active Directory falhar ou precisa ser restaurado. Se a senha de administrador de rede e a senha DSRM forem diferentes, DSRM não será carregado.  
  
 Durante uma instalação limpa e pela primeira vez, do Windows Server Essentials, o programa define a senha DSRM como a senha de conta de administrador da rede que você especifica durante a instalação ou no arquivo de resposta da migração. Quando você alterar sua senha de administrador de rede (como recomendado normalmente cada 60 dias para a segurança do servidor maior), a alteração de senha não é encaminhada ao DSRM. Isso resulta em uma incompatibilidade de senha. Se isso ocorrer, você pode usar as seguintes soluções para manualmente ou sincronizar automaticamente a senha de s do administrador de rede com a senha DSRM.  
  
##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Para sincronizar manualmente a senha DSRM para uma conta de administrador de rede  
  
1.  Em um prompt de comando, execute `ntdsutil.exe` para abrir a ferramenta ntdsutil.  
  
2.  Para redefinir a senha DSRM, digite **definir dsrm senha**.  
  
3.  Para sincronizar a senha DSRM em um controlador de domínio com a conta de s de administrador de rede atual, digite:  
  
     **sincronização de conta de domínio** *< current_network_administrator_account >*, e pressione Enter.  
  
 Porque você alterará periodicamente a senha da conta de administrador de rede, para garantir que a senha DSRM é sempre o mesmo que a senha atual do administrador de rede, recomendamos que você crie uma tarefa agendada para automaticamente sincronize a senha DSRM para a senha de administrador de rede diariamente.  
  
##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Para sincronizar automaticamente a senha DSRM para uma conta de administrador de rede  
  
1.  No servidor, abra **ferramentas administrativas**e clique duas vezes em **Agendador de tarefas**.  
  
2.  No Agendador de tarefas **ações** painel, clique em **criar tarefa**.  
  
3.  No **nome** caixa de texto, digite um nome para a tarefa, como **sincronização automática DSRM senha**e, em seguida, selecione o **executar com privilégios mais altos** opção.  
  
4.  Defina quando a tarefa deve ser executado:  
  
    1.  No **criar tarefa** caixa de diálogo, clique no **gatilhos** guia e, em seguida, clique em **nova**.  
  
    2.  No **novo disparador** caixa de diálogo, selecione a opção de recorrência, especifique o intervalo de recorrência e escolha uma hora de início.  
  
        > [!NOTE]
        >  Como prática recomendada, você deve definir a tarefa seja executada diariamente durante fora do expediente.  
  
    3.  Clique em **Okey** para salvar suas alterações e retornar para o **criar tarefa** caixa de diálogo.  
  
5.  Defina as ações de tarefa:  
  
    1.  Clique no **ações** guia e, em seguida, clique em **nova**. O **nova ação** caixa de diálogo aparece.  
  
    2.  No **ação** listar, clique em **iniciar um programa**e, em seguida, navegue até **C:\WINDOWS\SYSTEM32\ntdsutil.exe**.  
  
    3.  No **adicionar argumentos**texto (opcional), digite o seguinte (você deve incluir as aspas): **definir dsrm sincronização de senha de conta de domínio SBS_network_administrator_account q q** onde *SBS_network_administrator_account* é o nome de conta de administrador s de rede atual.  
  
6.  Clique em **Okey** duas vezes para salvar a tarefa e feche o **criar tarefa** caixa de diálogo. A nova tarefa aparece no **tarefas ativas** seção **Agendador**.  
  
###  <a name="BKMK_Access8"></a>Dar permissão da área de trabalho remota a contas de usuário  
 Na instalação padrão do Windows Server Essentials, os usuários da rede não têm permissão para estabelecer uma conexão remota a computadores ou outros recursos na rede.  
  
 Antes dos usuários da rede podem estabelecer uma conexão remota aos recursos de rede, primeiro você deve configurar acesso em qualquer local. Depois de configurar o acesso em qualquer lugar, os usuários podem acessar arquivos, aplicativos e computadores em sua rede de escritório de um dispositivo em qualquer local com uma conexão de Internet.  
  
 Configurar Assistente de acesso em qualquer lugar permitirá que você habilite dois métodos de acesso remoto:  
  
-   Rede virtual privada (VPN)  
  
-   Acesso via Web remoto  
  
 Quando você executa o assistente, você pode optar por permitir o acesso em qualquer lugar para todas as contas de usuário atual e recém-adicionado.  
  
 Para configurar o acesso em qualquer lugar, abra o painel **Home** página, clique em **instalação**e clique em **configurar o acesso em qualquer lugar**.  
  
 Para obter mais informações sobre acesso em qualquer lugar, consulte [gerenciar o acesso em qualquer local](Manage-Anywhere-Access-in-Windows-Server-Essentials.md).  
  
###  <a name="BKMK_Access9"></a>Permitir que os usuários acessem os recursos no servidor  
  Esta seção se aplica a um servidor que executa o Windows Server Essentials ou o Windows Server Essentials, ou para um servidor que executa o Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter com a função de experiência do Windows Server Essentials instalada.  
  
 Se você quer que os usuários usam o acesso remoto, e/ou ter contas de usuário individual, depois de concluir a conexão de um computador para o servidor, você pode criar nova rede contas de usuário para que os usuários do computador em rede no servidor usando o Dashboard. Para obter mais informações sobre como criar uma conta de usuário, consulte [adicionar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1). Depois de criar as contas de usuário, você deve fornecer as informações de nome e a senha de usuário rede para os usuários do computador cliente para que eles possam acessar recursos no servidor por meio da barra inicial.  
  
 Para cada conta de usuário que você crie você pode definir acesso para o seguinte por meio do usuário propriedades da conta:  
  
-   **Pastas compartilhadas**.  Por padrão, os administradores de rede têm **leitura/gravação** têm permissão para todas as pastas compartilhadas e contas de usuário padrão **somente leitura** permissões para a pasta da empresa. Se o streaming de mídia estiver habilitado, você pode atribuir permissões de acesso para contas de usuário padrão individuais para as seguintes pastas compartilhadas: **música**, **imagens**, **TV gravada**, e **vídeos**. Você pode definir permissões para contas de usuário acessar pastas compartilhadas no **pastas compartilhadas** guia das propriedades de conta de usuário.  
  
-   **Acesso em qualquer lugar**.  Por padrão, os administradores de rede podem usar VPN ou acesso via Web remoto aos recursos do servidor de acesso. Para contas de usuário padrão, você deve definir permissões da conta de usuário a **acesso em qualquer local** guia.  
  
-   **Acesso ao computador**.  Por padrão, os administradores de rede podem acessar todos os computadores na rede. No entanto, para contas de usuário padrão você pode definir permissões da conta de usuário individual para acessar computadores na rede no **acesso ao computador** guia das propriedades de conta de usuário.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>Para editar as propriedades de conta de usuário no Windows Server Essentials 2012 R2  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja editar.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **exibir as propriedades da conta**.  
  
5.  No **< usuário Account\ > propriedades**, faça o seguinte:  
  
    1.  Sobre o **pastas compartilhadas** guia, defina as permissões de pasta apropriada para cada pasta compartilhada, conforme necessário.  
  
    2.  Sobre o **acesso em qualquer local** guia:  
  
        1.  Para permitir que um usuário se conectar ao servidor usando VPN, selecione o **permitir rede Virtual privada (VPN)** caixa de seleção.  
  
        2.  Para permitir que um usuário se conectar ao servidor usando o acesso via Web remoto, selecione o **permitir acesso remoto via Web e acesso a aplicativos web services** caixa de seleção.  
  
    3.  Sobre o **acesso ao computador** , selecione os computadores da rede que você gostaria que o usuário tenha acesso a.  
  
##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>Para editar as propriedades de conta de usuário no Windows Server Essentials 2012  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja editar.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **propriedades**.  
  
5.  No **< usuário Account\ > propriedades**, faça o seguinte:  
  
    1.  Sobre o **geral** , selecione **usuário pode exibir alertas de integridade de rede** se a conta de usuário precisa acessar relatórios de integridade de rede.  
  
    2.  Sobre o **pastas compartilhadas** guia, defina as permissões de pasta apropriada para cada pasta compartilhada, conforme necessário.  
  
    3.  Sobre o **acesso em qualquer local** guia:  
  
        1.  Para permitir que um usuário se conectar ao servidor usando VPN, selecione o **permitir rede Virtual privada (VPN)** caixa de seleção.  
  
        2.  Para permitir que um usuário se conectar ao servidor usando o acesso via Web remoto, selecione o **permitir acesso remoto via Web e acesso a aplicativos web services** caixa de seleção.  
  
    4.  Sobre o **acesso ao computador** , selecione os computadores da rede que você gostaria que o usuário tenha acesso a.  
  
###  <a name="BKMK_Access10"></a>Alterar as permissões de acesso remoto para uma conta de usuário  
 Um usuário pode acessar recursos localizados no servidor de um local remoto por meio de uma rede virtual privada (VPN), acesso via Web remoto ou outros aplicativos de serviços da web. Por padrão, as permissões de acesso remoto estiverem ativadas para os usuários da rede ao configurar o acesso em qualquer lugar no Windows Server Essentials usando o Dashboard.  
  
##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>Para alterar as permissões de acesso remoto para uma conta de usuário  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja alterar.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **exibir as propriedades da conta**. O **propriedades** página para a conta de usuário é exibido.  
  
5.  Sobre o **acesso em qualquer local** guia, faça o seguinte:  
  
    -   Selecione o **permitir rede Virtual privada (VPN)** caixa de seleção Permitir que um usuário se conectar ao servidor usando VPN.  
  
    -   Selecione o **permitir acesso remoto via Web e acesso a aplicativos web services** caixa de seleção Permitir que um usuário se conectar ao servidor usando o acesso via Web remoto.  
  
6.  Clique em **aplicar**e clique em **Okey**.  
  
###  <a name="BKMK_Access11"></a>Alterar as permissões de rede virtual privada para uma conta de usuário  
 Você pode usar uma rede virtual privada (VPN) para se conectar ao Windows Server Essentials e acessar todos os recursos que são armazenados no servidor. Isso é especialmente útil se você tiver um computador cliente que é configurado com contas de rede que podem ser usadas para se conectar a um servidor Windows Server Essentials hospedado por meio de uma conexão VPN. Todas as contas de usuário recém criada no servidor Windows Server Essentials hospedado devem usar VPN para fazer logon no computador cliente pela primeira vez.  
  
##### <a name="to-change-vpn-permissions-for-network-users"></a>Para alterar as permissões de VPN para usuários de rede  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário ao qual você deseja conceder permissões para acessar a área de trabalho remota.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **propriedades**.  
  
5.  No **< usuário Account\ > propriedades**, clique no **acesso em qualquer local** guia.  
  
6.  Sobre o **acesso em qualquer local** tab, para permitir que um usuário se conectar ao servidor usando VPN, selecione o **permitir rede Virtual privada (VPN)** caixa de seleção.  
  
7.  Clique em **aplicar**e clique em **Okey**.  
  
###  <a name="BKMK_Access12"></a>Alterar o acesso a pastas compartilhadas internas para uma conta de usuário  
 Você pode gerenciar o acesso a todas as pastas compartilhadas no servidor, usando as tarefas no **pastas de servidor** guia do painel. Por padrão, as seguintes pastas de servidor são criadas quando você instala o Windows Server Essentials:  
  
-   **Backups do computador cliente**.  Usado para armazenar os backups do computador cliente criados pelo backup do Windows Server. Essa pasta de servidor não é compartilhada.  
  
-   **Empresa**.  Usado para armazenar e acessar documentos relacionados à sua organização pelos usuários da rede.  
  
-   **Backups do histórico de arquivos**.  Por padrão, o Windows Server Essentials armazena os backups de arquivo criados usando o histórico de arquivos. Essa pasta de servidor não é compartilhada.  
  
-   **Redirecionamento de pasta**.  Usado para armazenar e acessar as pastas que estão configuradas para redirecionamento de pasta pelos usuários da rede. Essa pasta de servidor não é compartilhada.  
  
-   **Música**.  Usado para armazenar e acessar arquivos de música pelos usuários da rede. Essa pasta é criada quando você ativar o compartilhamento de mídia.  
  
-   **Imagens**.  Usado para armazenar e acessar fotos pelos usuários da rede. Essa pasta é criada quando você ativar o compartilhamento de mídia.  
  
-   **Programas de TV gravados**.  Usado para armazenar e acessar programas de TV gravados pelos usuários da rede. Essa pasta é criada quando você ativar o compartilhamento de mídia.  
  
-   **Vídeos**.  Usado para armazenar e acessar vídeos pelos usuários da rede. Essa pasta é criada quando você ativar o compartilhamento de mídia.  
  
-   **Os usuários**.  Usado para armazenar e acessar arquivos por usuários de rede. Uma pasta específica do usuário é gerada automaticamente no **usuários** pasta de servidor para cada conta de usuário de rede que você criar.  
  
##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>Para alterar o acesso a uma pasta compartilhada para uma conta de usuário  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Clique em **armazenamento**e clique em **pastas de servidor**.  
  
3.  Navegue até e selecione a pasta de servidor para o qual você deseja modificar permissões.  
  
4.  No painel de tarefas, clique em **exibir as propriedades da pasta**.  
  
5.  Em **< FolderName\ > propriedades**, clique em **compartilhamento**, selecione o nível de acesso do usuário apropriado para as contas de usuário listada e, em seguida, clique em **aplicar**.  
  
    > [!NOTE]
    >  Você não pode modificar as permissões de compartilhamento **Backups de histórico de arquivos**, **redirecionamento de pasta**, e **usuários** pastas do servidor. Portanto, as propriedades da pasta dessas pastas de servidor não incluem um **compartilhamento** guia.  
  
###  <a name="BKMK_Access13"></a>Permitir que as contas de usuário estabelecer uma sessão da área de trabalho remota em seus computadores  
  Esta seção se aplica a um servidor que executa o Windows Server Essentials ou o Windows Server Essentials, ou para um servidor que executa o Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter com a função de experiência do Windows Server Essentials instalada.  
  
 O administrador da rede pode conceder permissões a usuários de rede que permitem que eles acessem seus computadores de rede de um local remoto.  
  
##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>Permitir que os usuários acessem seus computadores de rede de um local remoto  
  
1.  Abra o painel do Windows Server Essentials.  
  
2.  Na barra de navegação, clique em **usuários**.  
  
3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja conceder permissões para acessar a área de trabalho remota.  
  
4.  No **< usuário Account\ > tarefas** painel, clique em **propriedades**.  
  
5.  No **< usuário Account\ > propriedades**, clique no **acesso ao computador** guia.  
  
6.  Selecione os computadores que você deseja ser capaz de acessar remotamente e, em seguida, clique nesta conta de usuário **Okey**.  
  
## <a name="see-also"></a>Consulte também  
  
-   [Gerenciar contas Online para os usuários](Manage-Online-Accounts-for-Users.md)  
  
-   [Se conectar](../use/Get-Connected-in-Windows-Server-Essentials.md)  
  
-   [Usar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)  
  
-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
