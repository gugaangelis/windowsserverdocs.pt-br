---
title: Gerenciar contas de usuário no Windows Server Essentials
description: Descreve como usar o Windows Server Essentials
ms.date: 10/03/2016
ms.topic: article
ms.assetid: 0d115697-532b-48c2-a659-9f889e235326
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 60b37cb2945ca12f03b27a4367edff6eb0fe4746
ms.sourcegitcommit: 34f9577ef32cbdc7ef96040caabc9d83517f9b79
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/08/2020
ms.locfileid: "89554479"
---
# <a name="manage-user-accounts-in-windows-server-essentials"></a>Gerenciar contas de usuário no Windows Server Essentials

>Aplica-se a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

A página Usuários do Painel do Windows Server Essentials centraliza informações e tarefas que ajudam você gerenciar as contas de usuário em sua rede de pequena empresa. Para obter uma visão geral do painel usuários, consulte [visão geral do painel](Overview-of-the-Dashboard-in-Windows-Server-Essentials.md).


##  <a name="managing-user-accounts"></a><a name="BKMK_ManageAccounts"></a> Gerenciando contas de usuário
 Os tópicos a seguir fornecem informações sobre como usar o Painel do Windows Server Essentials para gerenciar as contas de usuário no servidor:

-   [Adicionar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1)

-   [Remover uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Remove)

-   [Exibir contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage3)

-   [Alterar o nome de exibição da conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage4)

-   [Ativar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage5)

-   [Desativar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage6)

-   [Entender as contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage7)

-   [Gerenciar contas de usuário usando o Painel](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage8)

###  <a name="add-a-user-account"></a><a name="BKMK_Manage1"></a> Adicionar uma conta de usuário
 Quando você adiciona uma conta de usuário, o usuário atribuído pode fazer logon na rede e você pode conceder ao usuário permissão para acessar recursos de rede, como pastas compartilhadas e o site de Acesso via Web remoto. O Windows Server Essentials inclui um Assistente de Adição de Conta de Usuário que o ajuda a:

-   Fornecer um nome e a senha da conta de usuário.

-   Definir a conta como administrador ou usuário padrão.

-   Selecionar quais pastas compartilhadas a conta de usuário pode acessar.

-   Especificar se a conta de usuário tem acesso remoto à rede.

-   Selecionar as opções de email, se aplicável.

-   Atribua uma conta do Microsoft Online Services (chamada de conta de Microsoft 365 no Windows Server Essentials), se aplicável.

-   Atribuir grupos de usuários (somente Windows Server Essentials).

> [!NOTE]
> - Não há suporte para caracteres não ASCII no Microsoft Azure Active Directory (Azure AD). Não use nenhum caractere não-ASCII em sua senha, se o servidor estiver integrado ao Azure AD.
>   -   As opções de email só estarão disponíveis se você instalar um suplemento que fornece o serviço de email.

##### <a name="to-add-a-user-account"></a>Para adicionar uma conta de usuário

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **Usuários**.

3.  No painel  **Tarefas de Usuários** , clique em **Adicionar uma conta de usuário**. O Assistente de Adição de Conta de Usuário é exibido.

4.  Siga as instruções para concluir o assistente.

###  <a name="remove-a-user-account"></a><a name="BKMK_Remove"></a> Remover uma conta de usuário
 Quando você opta por remover uma conta de usuário do servidor, um assistente exclui a conta selecionada. Por esse motivo, você não pode usar a conta para fazer logon na rede ou acessar os recursos de rede. Como opção, você também pode excluir os arquivos da conta de usuário ao mesmo tempo em que remover a conta. Caso não deseje remover permanentemente a conta de usuário, você pode desativar a conta para suspender o acesso aos recursos da rede.

> [!IMPORTANT]
>  Se uma conta de usuário tiver uma conta online da Microsoft atribuída, quando você remover a conta de usuário, a conta online também será removida do Microsoft Online Services e os dados do usuário, incluindo email, estão sujeitos a políticas de retenção de dados no Microsoft Online Services. Se você deseja manter os dados de usuário da conta online, desative a conta de usuário em vez de removê-la. Para obter mais informações, consulte [gerenciar contas online para usuários](Manage-Online-Accounts-for-Users.md).

##### <a name="to-remove-a-user-account"></a>Para remover uma conta de usuário

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **Usuários**.

3.  Na lista de contas de usuário, selecione a conta de usuário que deseja remover.

4.  No painel ** \> tarefas da conta de usuário do<** , clique em **remover a conta de usuário**. O Assistente de Exclusão de Conta de Usuário é exibido.

5.  Na página deseja **manter os arquivos?** do assistente, você pode optar por excluir os arquivos do usuário, incluindo backups de histórico de arquivos e a pasta redirecionada para a conta de usuário. Para manter os arquivos do usuário, deixe a caixa de seleção vazia. Depois de fazer sua escolha, clique em **Avançar**.

6.  Clique em **Excluir conta**.

> [!NOTE]
>  Depois de você remover uma conta de usuário, a conta deixará de aparecer na lista de contas de usuário. Se você optar por excluir os arquivos, o servidor excluirá permanentemente a pasta do usuário da pasta de servidores de **usuários** e da pasta de servidor **backups de histórico de arquivos** .
>
>  Se você tiver um provedor de email integrado, a conta de email atribuída à conta de usuário também será removida.

###  <a name="view-user-accounts"></a><a name="BKMK_Manage3"></a> Exibir contas de usuário
 A seção **Usuários** do Painel do Windows Server Essentials exibe uma lista de contas de usuário de rede. A lista também fornece informações adicionais sobre cada conta.

##### <a name="to-view-a-list-of-user-accounts"></a>Para exibir uma lista de contas de usuário

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação principal, clique em **Usuários**.

3.  O Painel exibe uma lista de contas de usuário atual.

##### <a name="to-view-or-change-properties-for-a-user-account"></a>Para exibir ou alterar as propriedades de uma conta de usuário

1.  Na lista de contas de usuário, selecione a conta para a qual você deseja exibir ou alterar as propriedades.

2.  No painel ** \> tarefas da conta de usuário do<** , clique em **exibir as propriedades da conta**. A página **Propriedades** da conta de usuário é exibida.

3.  Clique na guia para exibir as propriedades da conta.

4.  Para salvar as alterações feitas nas propriedades da conta de usuário, clique em **Aplicar**.

###  <a name="change-the-display-name-for-the-user-account"></a><a name="BKMK_Manage4"></a> Alterar o nome de exibição da conta de usuário
 O nome de exibição é o nome que aparece na coluna **Nome** da página **Usuários** do Painel. Alterar o nome de exibição não altera o nome de logon ou de entrada da uma conta de usuário.

##### <a name="to-change-the-display-name-for-a-user-account"></a>Para alterar o nome de exibição de uma conta de usuário

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **Usuários**.

3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja alterar.

4.  No painel ** \> tarefas da conta de usuário do<** , clique em **exibir as propriedades da conta**. A página **Propriedades** da conta de usuário é exibida.

5.  Na guia **Geral**, digite um novo **Nome** e **Sobrenome** para a conta de usuário e clique em **OK**.

     O novo nome de exibição aparece na lista de contas de usuário.

###  <a name="activate-a-user-account"></a><a name="BKMK_Manage5"></a> Ativar uma conta de usuário
 Quando você ativa uma conta de usuário, o usuário atribuído pode fazer logon na rede e acessar recursos de rede para os quais a conta tem permissão, como pastas compartilhadas e o Acesso via Web remoto.

> [!NOTE]
>  Só é possível ativar uma conta de usuário que está desativada. Você não pode ativar uma conta de usuário após ela ser removida do servidor.

##### <a name="to-activate-a-user-account"></a>Para ativar uma conta de usuário

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **Usuários**.

3.  Na exibição de lista, selecione a conta de usuário que você deseja ativar.

4.  No painel ** \> tarefas da conta de usuário do<** , clique em **ativar a conta de usuário**.

5.  Na janela de confirmação, clique em **Sim** para confirmar a ação.

> [!NOTE]
>  Depois que você ativar uma conta de usuário, o status da conta será exibido como **Ativa**. A conta de usuário recupera os mesmos direitos de acesso que foram atribuídos antes da desativação de conta.
>
>  Se você tiver um provedor de email integrado, a conta de email atribuída à conta de usuário também será ativada.

###  <a name="deactivate-a-user-account"></a><a name="BKMK_Manage6"></a> Desativar uma conta de usuário
 Quando você desativa uma conta de usuário, o acesso da conta ao servidor é suspenso temporariamente. Por isso, o usuário atribuído não pode usar a conta para acessar recursos de rede como pastas compartilhadas ou o site de Acesso Remoto via Web até você ativar a conta.

 Se a conta de usuário tiver uma conta da Microsoft online atribuída, a conta online também será desativada. O usuário não pode usar recursos no Microsoft 365 e outros serviços online nos quais você se inscreveu, mas os dados do usuário, incluindo email, são mantidos no Microsoft Online Services.

> [!NOTE]
>  Você só pode desativar uma conta de usuário que está ativa no momento.

##### <a name="to-deactivate-a-user-account"></a>Para desativar uma conta de usuário

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **Usuários**.

3.  Na exibição de lista, selecione a conta de usuário que você deseja desativar.

4.  No painel ** \> tarefas da conta de usuário<** , clique em **desativar a conta de usuário**.

5.  Na janela de confirmação, clique em **Sim** para confirmar a ação.

> [!NOTE]
>  Após você desativar uma conta de usuário, o status da conta será exibido como **Inativo**.
>
>  Se você tiver um provedor de email integrado, a conta de email atribuída à conta de usuário também será desativada.

###  <a name="understand-user-accounts"></a><a name="BKMK_Manage7"></a> Entender as contas de usuário
 Uma conta de usuário fornece informações importantes para o Windows Server Essentials, que permite acessar informações armazenadas no servidor e possibilita que usuários individuais criem e gerenciem seus arquivos e configurações. Os usuários podem fazer logon em qualquer computador da rede se tiverem uma conta de usuário do Windows Server Essentials e têm permissão para acessar um computador. Os usuários acessam suas contas de usuário com seu nome de usuário e senha.

 Há dois tipos principais de contas de usuário. Cada tipo fornece aos usuários um nível diferente de controle sobre o computador:

-   Contas **Padrão** são destinadas à computação cotidiana. A conta padrão ajuda a proteger sua rede, impedindo que os usuários façam alterações que afetam outros usuários, como excluir arquivos ou alterar as configurações de rede.

-   Contas de **Administrador** oferecem mais controle sobre uma rede de computadores. Você deve atribuir o tipo de conta de administrador somente quando for necessário.

###  <a name="manage-user-accounts-using-the-dashboard"></a><a name="BKMK_Manage8"></a> Gerenciar contas de usuário usando o painel
 O Windows Server Essentials torna possível executar tarefas administrativas comuns usando o Painel do Windows Server Essentials. Por padrão, a página **usuários** do painel inclui duas guias: **usuários** e **grupos de usuários**.

> [!NOTE]
> - Se você integrar o servidor que está executando o Windows Server Essentials com o Microsoft 365, uma nova guia chamada **grupos de distribuição** também será adicionada na página **usuários** do painel.
>   -   No Windows Server Essentials, a página **usuários** do painel inclui apenas uma única guia- **usuários**.

 A guia **Usuários** inclui o seguinte:

- Uma lista de contas de usuário, que exibe:

  -   O nome de usuário.

  -   O nome de Logon da conta de usuário.

  -   Se a conta de usuário tem permissão de Acesso em qualquer lugar. A permissão de Acesso em qualquer lugar para uma conta de usuário é **Permitido** ou **Não permitido**.

  -   Se o histórico de arquivos para esta conta de usuário é gerenciado pelo servidor que executa o Windows Server Essentials. O status do histórico de arquivos para uma conta de usuário é **Gerenciado** ou **Não gerenciado**.

  -   O nível de acesso que é atribuído à conta de usuário. Você pode atribuir o acesso de **Usuário padrão** ou **Administrador** a uma conta de usuário.

  -   O status da conta de usuário. Uma conta de usuário pode ser **Ativa**, **Inativa** ou **Incompleta**.

  -   No Windows Server Essentials, se o servidor estiver integrado ao Microsoft 365 ou ao Windows Intune, a conta online da Microsoft será exibida.

  -   No Windows Server Essentials, se o servidor estiver integrado ao Microsoft 365, o status da conta (conhecido no Windows Server Essentials como a conta online da Microsoft) para a conta de usuário será exibido.

- Um painel de detalhes com informações adicionais sobre uma conta de usuário selecionada.

- Um painel de tarefas que inclui:

  -   Um conjunto de tarefas administrativas de conta de usuário, como exibir e remover as contas de usuário e alterar senhas.

  -   Tarefas que permitem definir globalmente ou alterar as configurações para todas as contas de usuário na rede.

  A tabela a seguir descreve as várias tarefas de conta de usuário que estão disponíveis na guia **usuários** . Algumas das tarefas são específicas da conta de usuário e ficam visíveis apenas quando você seleciona uma conta de usuário na lista.

> [!NOTE]
>  Se você integrar Microsoft 365 com o Windows Server Essentials, tarefas adicionais ficarão disponíveis. Para obter mais informações, consulte [gerenciar contas online para usuários](Manage-Online-Accounts-for-Users.md).

### <a name="user-account-tasks-in-the-dashboard"></a>Tarefas de conta de usuário no Painel

|Nome da tarefa|Descrição|
|---------------|-----------------|
|Exibir as propriedades de conta|Permite exibir e alterar as propriedades da conta de usuário selecionada e especificar as permissões de acesso a pastas para a conta.|
|Desativar a conta de usuário|Uma conta de usuário que está desativada não consegue fazer logon de rede ou acessar recursos de rede, como pastas compartilhadas ou impressoras.|
|Ativar a conta de usuário|Uma conta de usuário ativada pode fazer logon na rede e acessar recursos de rede, conforme definido pelas permissões da conta.|
|Remover a conta de usuário|Permite que você remova a conta de usuário selecionada.|
|Alterar a senha da conta de usuário|Permite que você redefina a senha de rede para a conta de usuário selecionada.|
|Adicionar uma conta de usuário|Inicia o Assistente de Adição de Conta de Usuário, que permite que você crie uma nova conta de usuário que tem acesso de usuário padrão ou de administrador.|
|Atribuir uma conta online da Microsoft|Adiciona uma conta online da Microsoft à conta de usuário da rede local que está selecionada.<br /><br /> Essa tarefa é exibida quando o servidor é integrado ao Microsoft serviços online, como Microsoft 365.|
|Adicionar contas online da Microsoft|Adiciona contas online da Microsoft e as associa a contas de usuário de rede local.<br /><br /> Essa tarefa é exibida quando o servidor é integrado ao Microsoft serviços online, como Microsoft 365.|
|Definir a política de senha|Permite que você altere os valores da política de senhas para sua rede.|
|Importar contas online da Microsoft|Realiza uma importação em massa de contas de serviços online da Microsoft na rede local.<br /><br /> Essa tarefa é exibida quando o servidor é integrado ao Microsoft serviços online, como Microsoft 365.|
|Atualizar|Atualiza a guia de usuários.<br /><br /> Essa tarefa é aplicável ao Windows Server Essentials.|
|Alterar as configurações do histórico de arquivos|Permite que você altere as configurações do histórico de arquivos, como a frequência de backup ou duração do backup.<br /><br /> Essa tarefa é aplicável ao Windows Server Essentials.|
|Exportar todas as conexões remotas|Cria um arquivo de formato .CSV de todas as conexões remotas ao servidor que ocorreram nos últimos 30 dias.|

##  <a name="managing-passwords-and-access"></a><a name="BKMK_ManageAccess"></a> Gerenciando senhas e acesso
 Os tópicos a seguir fornecem informações sobre como usar o Painel do Windows Server Essentials para gerenciar as senhas de contas de usuário e o acesso dos usuários às pastas compartilhadas no servidor:

-   [Alterar ou redefinir a senha de uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access1)

-   [O que você deve saber sobre políticas de senha](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access3)

-   [Alterar a política de senha](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access4)

-   [Nível de acesso a pastas compartilhadas](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access5)

-   [Manter e gerenciar o acesso aos arquivos de contas de usuário removidas](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access6)

-   [Sincronizar a senha DSRM com a senha de administrador de rede](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access7)

-   [Conceder permissão de área de trabalho remota a contas de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access8)

-   [Permitir que os usuários acessem os recursos do servidor](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access9)

-   [Alterar as permissões de acesso remoto para uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access10)

-   [Alterar as permissões de rede privada virtual de uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access11)

-   [Alterar o acesso a pastas compartilhadas internas de uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access12)

-   [Permitir que as contas de usuário estabeleçam uma sessão de área de trabalho remota em seu computador](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Access13)

###  <a name="change-or-reset-the-password-for-a-user-account"></a><a name="BKMK_Access1"></a> Alterar ou redefinir a senha de uma conta de usuário
 Para alterar ou redefinir a senha de uma conta de usuário, siga estas etapas.

##### <a name="to-reset-the-password-for-a-user-account"></a>Para redefinir a senha de uma conta de usuário

1. Abra o Painel do Windows Server Essentials.

2. Na barra de navegação, clique em **Usuários**.

3. Na lista de contas de usuário, selecione a conta de usuário que você deseja redefinir.

4. No painel ** \> tarefas da conta de usuário do<** , clique em **alterar a senha da conta de usuário**. O Assistente de Alteração de Senha de Conta de Usuário é exibido.

5. Digite uma nova senha para a conta de usuário e, em seguida, digite a senha novamente para confirmá-la.

6. Clique em **Alterar senha**.

7. Fornece a nova senha do usuário.

   > [!IMPORTANT]
   > - Você não poderá alterar sua senha se a política de senha para a sua conta tiver sido definida como **As senhas nunca expiram**.
   >   -   Não há suporte para caracteres não ASCII no Azure AD. Portanto, se o servidor estiver integrado ao Azure AD, não use caracteres não ASCII em sua senha.
   >   -   Se uma conta online da Microsoft (conhecida no Windows Server Essentials como uma conta de Microsoft 365) for atribuída ao usuário, a senha será sincronizada com a senha da conta online. O usuário usará a nova senha para entrar no servidor ou entrar no Microsoft 365. Para obter mais informações, consulte [gerenciar contas online para usuários](Manage-Online-Accounts-for-Users.md).

###  <a name="what-you-should-know-about-password-policies"></a><a name="BKMK_Access3"></a> O que você deve saber sobre as políticas de senha
 A política de senha é um conjunto de regras que definem como os usuários criam e usam senhas. A política ajuda a evitar acesso não autorizado aos dados do usuário e a outras informações armazenadas no servidor. A política de senha é aplicada a todas as contas de usuário que acessam a rede.

 A política de senha do Windows Server Essentials consiste em três elementos principais, da seguinte maneira:

- **Comprimento da senha**.  Quanto mais longa, mais segura é a senha. Senhas em branco não são seguras.

- **Complexidade de senha**.  Senhas complexas contêm uma mistura de letras maiúsculas e minúsculas (a-z, A-Z), números básicos (0-9) e símbolos não alfabéticos (como;!, @, #, _,-). Senhas complexas são muito menos suscetíveis a acesso não autorizado. Senhas que contêm os nomes de usuário, datas de nascimento ou outras informações pessoais não fornecem segurança adequada.

- **Duração da senha**.  O Windows Server Essentials exige que os usuários alterem suas senhas pelo menos uma vez a cada 180 dias. Como opção, você pode optar por ter senhas nunca expirar.

  Para tornar mais fácil implementar uma política de senha no computador da rede, o Windows Server Essentials fornece uma ferramenta simples que permite que você defina ou altere a política de senha para qualquer um dos seguintes quatro perfis predefinidos de política:

- **Fraca**.  Os usuários podem especificar qualquer senha que não esteja em branco.

- **Média**.  Essas senhas devem conter pelo menos 5 caracteres. Uma senha complexa não é obrigatória.

- **Média Forte**.  As senhas devem conter pelo menos 5 caracteres e devem incluir letras, números e símbolos.

- **Forte**.  As senhas devem conter pelo menos 7 caracteres e devem incluir letras, números e símbolos. Essas senhas são mais seguras, mas podem ser mais difíceis para os usuários memorizarem.

  > [!NOTE]
  >  A senhas não podem conter o endereço de email ou o nome de usuário.
  >
  >  Se você se integrar com o Microsoft 365, a integração imporá a política de senha **forte** e atualizará a política para incluir os seguintes requisitos:
  >
  > - As senhas devem conter 8 16 caracteres.
  >   -   As senhas não podem conter um espaço ou o nome de email Microsoft 365.

  Por padrão, a instalação do servidor define a política de senha padrão a opção **Forte**.

###  <a name="change-the-password-policy"></a><a name="BKMK_Access4"></a> Alterar a política de senha
 Use o procedimento a seguir para definir ou alterar a política de senha para qualquer um dos quatro perfis predefinidos de política.

##### <a name="to-change-the-password-policy"></a>Para alterar a política de senha

1.  Abra o Painel do Windows Server Essentials e, em seguida, clique em **Usuários**.

2.  No painel **Tarefas de usuários**, clique em **Definir a política de senha**.

3.  Na tela **Alterar a política de senha**, defina o nível de força da senha movendo o controle deslizante.

     A Microsoft recomenda que você defina a força da senha como **Forte**.

    > [!NOTE]
    >  Como opção, você também pode selecionar **As senhas nunca expiram**. Essa configuração é menos segura, e portanto não é recomendada.

4.  Clique em **Alterar política**.

###  <a name="level-of-access-to-shared-folders"></a><a name="BKMK_Access5"></a> Nível de acesso a pastas compartilhadas
 Como prática recomendada, você deve atribuir as permissões mais restritivas possíveis que ainda permitam que os usuários executem as tarefas necessárias.

 Você tem três configurações de acesso disponíveis para as pastas compartilhadas no servidor:

-   **Leitura/gravação**.  Escolha essa configuração para permitir que a conta de usuário crie, altere e exclua arquivos na pasta compartilhada.

-   **Somente leitura**.  Escolha essa configuração para permitir que a conta de usuário somente leia arquivos na pasta compartilhada. Contas de usuário com acesso somente leitura não podem criar, alterar ou excluir os arquivos na pasta compartilhada.

-   **Sem acesso**.  Escolha essa opção se você não quiser que a conta de usuário acesse arquivos na pasta compartilhada.

###  <a name="retain-and-manage-access-to-files-for-removed-user-accounts"></a><a name="BKMK_Access6"></a> Reter e gerenciar o acesso a arquivos para contas de usuário removidas
 O administrador de rede pode remover uma conta de usuário e optar por manter os arquivos do usuário para uso futuro. Neste cenário, a conta de usuário removida não pode mais ser usada para entrar na rede. No entanto, os arquivos desse usuário serão salvos em uma pasta compartilhada, que pode ser compartilhada com outro usuário.

> [!IMPORTANT]
>  Lembre-se de que se você remover uma conta de usuário que tiver uma conta da Microsoft online atribuída, a conta online também será removida e os dados do usuário, incluindo email, estarão sujeitos às políticas de retenção de dados do Microsoft Online Services. Para manter os dados do usuário da conta online, desative a conta de usuário em vez de removê-lo. Para obter mais informações, consulte [gerenciar contas online para usuários](Manage-Online-Accounts-for-Users.md).

##### <a name="to-remove-a-user-account-but-retain-access-to-the-users-files"></a>Para remover uma conta de usuário, mas manter o acesso aos arquivos do usuário

1. Abra o Painel do Windows Server Essentials.

2. Na barra de navegação, clique em **Usuários**.

3. Na lista de contas de usuário, selecione a conta de usuário que deseja remover.

4. No painel ** \> tarefas da conta de usuário do<** , clique em **remover a conta de usuário**. O Assistente de Exclusão de Conta de Usuário é exibido.

5. Na página **Deseja manter os arquivos?**, certifique-se de que a caixa de seleção **Excluir os arquivos, incluindo os backups do Histórico de Arquivos e a pasta redirecionada, desta conta de usuário** não esteja marcada e clique em **Avançar**.

    Uma página de confirmação aparece, informando que você está excluindo a conta e mantendo os arquivos.

6. Clique em **Excluir conta** para remover a conta de usuário.

   Depois que a conta de usuário for removida, o administrador pode fornecer a outra conta de usuário acesso à pasta compartilhada.

##### <a name="to-give-a-user-account-permission-to-access-a-shared-folder"></a>Para dar a uma conta de usuário permissão para acessar uma pasta compartilhada

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **Armazenamento** e clique na guia **Pastas do Servidor**.

3.  Na lista de pastas, selecione a pasta **Usuários**.

4.  No painel **Tarefas de Usuários**, clique em **Abrir a pasta**. O Windows Explorer é aberto e exibe o conteúdo da pasta **Usuários**.

5.  Clique com o botão direito do mouse na pasta da conta de usuário que deseja compartilhar e, em seguida, clique em **Propriedades**.

6.  Em **<\> Propriedades da conta de usuário**, clique na guia **compartilhamento** e, em seguida, clique em **compartilhar**.

7.  Na janela **Compartilhamento de arquivos**, digite ou selecione o nome da conta de usuário com que você deseja compartilhar a pasta e, em seguida, clique em **Adicionar**.

8.  Escolha o **Nível de permissão** que você deseja que a conta de usuário tenha e, em seguida, clique em **Compartilhamento**.

###  <a name="synchronize-the-dsrm-password-with-the-network-administrator-password"></a><a name="BKMK_Access7"></a> Sincronizar a senha do DSRM com a senha de administrador de rede
 Modo de Restauração dos Serviços de Diretório (DSRM) é um modo especial de inicialização para reparo ou recuperação do Active Directory. O sistema operacional usa o DSRM para fazer logon no computador se o Active Directory falhar ou precisar ser restaurado. Se a senha de administrador de rede e a senha do DSRM forem diferentes, o DSRM não será carregado.

 Durante uma instalação limpa do Windows Server Essentials, feita pela primeira vez, o programa define a senha do DSRM como a senha da conta do administrador de rede que você especificar durante a instalação ou que estiver no arquivo de resposta da migração. Quando você altera a senha de administrador de rede (como recomendado normalmente cada 60 dias, para maior segurança do servidor), a alteração de senha não será refletida no DSRM. Isso resulta em uma incompatibilidade de senhas. Se isso ocorrer, você poderá usar as soluções a seguir para sincronizar manualmente ou automaticamente a senha do administrador de rede com a senha do DSRM.

##### <a name="to-manually-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Para sincronizar manualmente a senha do DSRM com uma conta de administrador de rede

1. Em um prompt de comando, execute `ntdsutil.exe` para abrir a ferramenta ntdsutil.

2. Para redefinir a senha do DSRM, digite **set dsrm password**.

3. Para sincronizar a senha do DSRM em um controlador de domínio com a conta do administrador de rede atual, digite:

    **sincronize a partir da conta de domínio** *<current_network_administrator_account>* e pressione Enter.

   Como você alterará periodicamente a senha da conta de administrador de rede, para garantir que a senha do DSRM seja sempre igual à senha atual do administrador de rede, recomendamos que você crie uma tarefa agendada para sincronizar automaticamente a senha do DSRM com a senha do administrador de rede diariamente.

##### <a name="to-automatically-synchronize-the-dsrm-password-to-a-network-administrator-account"></a>Para sincronizar automaticamente a senha do DSRM com uma conta de administrador de rede

1.  No servidor, abra **Ferramentas Administrativas** e clique duas vezes em **Agendador de Tarefas**.

2.  No painel **Ações** do Agendador de Tarefas, clique em **Criar Tarefa**.

3.  Na caixa de texto **Nome**, digite um nome para a tarefa, como **Sincronização automática da senha do DSM** e selecione a opção **Executar com privilégios mais altos**.

4.  Defina quando a tarefa deve ser executada:

    1.  Na caixa de diálogo **Criar Tarefa**, clique na guia **Disparadores** e, em seguida, clique em **Novo**.

    2.  Na caixa de diálogo **Novo gatilho**, selecione a opção de recorrência, especifique o intervalo de recorrência e escolha uma hora de início.

        > [!NOTE]
        >  Como prática recomendada, você deve definir que a tarefa seja executada diariamente, não horário comercial.

    3.  Clique em **OK** para salvar suas alterações e retornar para a caixa de diálogo **Criar Tarefa**.

5.  Defina as ações da tarefa:

    1.  Clique na guia **Ações** e, em seguida, clique em **Nova**. A caixa de diálogo **Nova ação** é exibida.

    2.  Na lista **Ação**, clique em **Iniciar um programa** e navegue até **C:\WINDOWS\SYSTEM32\ntdsutil.exe**.

    3.  Na caixa de texto **adicionar argumentos**(opcional), digite o seguinte (você deve incluir as aspas): **defina a sincronização de senha do DSRM da conta de domínio SBS_network_administrator_account q q** , em que *SBS_network_administrator_account* é o nome da conta do administrador de rede atual.

6.  Clique em **OK** duas vezes para salvar a tarefa e fechar a caixa de diálogo **Criar Tarefa**. A nova tarefa aparece na seção **Tarefas ativas** do **Agendador de Tarefas**.

###  <a name="give-user-accounts-remote-desktop-permission"></a><a name="BKMK_Access8"></a> Conceder permissão de área de trabalho remota às contas de usuário
 Na instalação padrão do Windows Server Essentials, os usuários da rede não têm permissão para estabelecer uma conexão remota com computadores ou outros recursos na rede.

 Para que os usuários da rede possam estabelecer uma conexão remota com recursos da rede, primeiro você deve configurar o Acesso em qualquer local. Após o Acesso em qualquer lugar ser configurado, os usuários podem acessar arquivos, aplicativos e computadores em sua rede de escritório a partir de um dispositivo em qualquer local com uma conexão à Internet.

 O Assistente de Acesso em Qualquer Local permite habilitar dois métodos de acesso remoto:

- Rede virtual privada (VPN)

- Acesso Remoto via Web

  Quando você executa o assistente, também é possível permitir acesso em qualquer lugar para todas as contas de usuário atuais e recém-adicionada.

  Para configurar o Acesso em qualquer lugar, abra a página de **Início** do Painel, clique em **INSTALAÇÃO** e clique em **Configurar Acesso em Qualquer Local**.

  Para obter mais informações sobre o acesso em qualquer lugar, consulte [gerenciar acesso em qualquer lugar](Manage-Anywhere-Access-in-Windows-Server-Essentials.md).

###  <a name="enable-users-to-access-resources-on-the-server"></a><a name="BKMK_Access9"></a> Permitir que os usuários acessem recursos no servidor
  Esta seção se aplica a um servidor que executa o Windows Server Essentials ou o Windows Server Essentials ou a um servidor executando o Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter com a função de experiência do Windows Server Essentials instalada.

 Se você deseja que os usuários usem acesso remoto e/ou tenham contas de usuário individuais, depois de concluir a conexão de um computador com o servidor, você pode criar novas contas de usuário da rede para os usuários do computador em rede no servidor usando o Painel. Para obter mais informações sobre como criar uma conta de usuário, consulte [Adicionar uma conta de usuário](Manage-User-Accounts-in-Windows-Server-Essentials.md#BKMK_Manage1). Depois de criar as contas de usuário, você deve fornecer as informações de nome de usuário e senha rede para os usuários do computador cliente, para que eles possam acessar recursos do servidor usando a Barra Inicial.

 Para cada conta de usuário que criar, você pode definir o acesso por meio das seguintes propriedades de conta de usuário:

-   **Pastas compartilhadas**.  Por padrão, os administradores de rede têm permissão de **Leitura/gravação** para todas as pastas compartilhadas, e as contas de usuário padrão têm permissões **Somente leitura** para a pasta da Empresa. Se o streaming de mídia estiver habilitado, você poderá atribuir permissões de acesso para contas de usuário padrão para as seguintes pastas compartilhadas: **Música**, **Imagens**, **TV gravada**, e **Vídeos**. Você pode definir permissões para contas de usuário acessarem pastas compartilhadas na guia **Pastas compartilhadas** das propriedades da conta do usuário.

-   **Acesso em qualquer lugar**.  Por padrão, os administradores de rede podem usar VPN ou Acesso Remoto via Web para acessar recursos do servidor. Para contas de usuário padrão, você deve definir permissões de conta de usuário na guia **Acesso em qualquer lugar**.

-   **Acesso ao computador**.  Por padrão, os administradores de rede podem acessar todos os computadores na rede. No entanto, para contas de usuário padrão você pode definir permissões de conta de usuário individual para acessar os computadores da rede na guia **Acesso ao computador** das propriedades da conta do usuário.

##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012-r2"></a>Para editar as propriedades de conta de usuário no Windows Server Essentials 2012 R2

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **USUÁRIOS**.

3.  Na lista de contas de usuário, selecione a conta de usuário que deseja remover.

4.  No painel ** \> tarefas da conta de usuário do<** , clique em **exibir as propriedades da conta**.

5.  Na **<\> Propriedades da conta de usuário**, faça o seguinte:

    1.  Na guia **Pastas compartilhadas**, defina as permissões de pasta apropriada para cada pasta compartilhada, conforme necessário.

    2.  Na guia **Acesso em qualquer lugar**:

        1.  Para permitir que um usuário se conecte ao servidor usando VPN, marque a caixa de seleção **Permitir VPN (rede privada virtual)**.

        2.  Para permitir que um usuário se conecte ao servidor usando o Acesso Remoto via Web, marque a caixa de seleção **Permitir Acesso Remoto via Web e acesso a aplicativos de serviços Web**.

    3.  Na guia **Acesso ao computador** , selecione os computadores da rede a que deseja que o usuário tenha.

##### <a name="to-edit-user-account-properties-in-windows-server-essentials-2012"></a>Para editar as propriedades de conta de usuário no Windows Server Essentials 2012

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **USUÁRIOS**.

3.  Na lista de contas de usuário, selecione a conta de usuário que deseja remover.

4.  No painel ** \> tarefas da conta de usuário<** , clique em **Propriedades**.

5.  Na **<\> Propriedades da conta de usuário**, faça o seguinte:

    1.  Na guia **Geral**, selecione **O usuário pode exibir alertas da integridade da rede** se a conta de usuário precisar acessar os relatórios de integridade da rede.

    2.  Na guia **Pastas compartilhadas**, defina as permissões de pasta apropriada para cada pasta compartilhada, conforme necessário.

    3.  Na guia **Acesso em qualquer lugar**:

        1.  Para permitir que um usuário se conecte ao servidor usando VPN, marque a caixa de seleção **Permitir VPN (rede privada virtual)**.

        2.  Para permitir que um usuário se conecte ao servidor usando o Acesso Remoto via Web, marque a caixa de seleção **Permitir Acesso Remoto via Web e acesso a aplicativos de serviços Web**.

    4.  Na guia **Acesso ao computador** , selecione os computadores da rede a que deseja que o usuário tenha.

###  <a name="change-remote-access-permissions-for-a-user-account"></a><a name="BKMK_Access10"></a> Alterar permissões de acesso remoto para uma conta de usuário
 Um usuário pode acessar os recursos localizados no servidor a partir de um local remoto usando uma rede privada virtual (VPN), Acesso Remoto via Web ou outros aplicativos de serviços Web. Por padrão, as permissões de acesso remoto estão ativadas para os usuários da rede quando você configura o acesso em qualquer local no Windows Server Essentials usando o Painel.

##### <a name="to-change-remote-access-permissions-for-a-user-account"></a>Para alterar permissões de acesso remoto para uma conta de usuário

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **Usuários**.

3.  Na lista de contas de usuário, selecione a conta de usuário que você deseja alterar.

4.  No painel ** \> tarefas da conta de usuário do<** , clique em **exibir as propriedades da conta**. A página **Propriedades** da conta de usuário é exibida.

5.  Na guia **Acesso em qualquer lugar**, faça o seguinte:

    -   Marque a caixa de seleção **Permitir VPN (rede privada virtual)** para permitir que um usuário se conecte ao servidor usando VPN.

    -   Marque a caixa de seleção **Permitir Acesso Remoto via Web e acesso a aplicativos de serviços Web** para permitir que um usuário se conecte ao servidor usando o Acesso Remoto via Web.

6.  Clique em **Aplicar** e em **OK**.

###  <a name="change-virtual-private-network-permissions-for-a-user-account"></a><a name="BKMK_Access11"></a> Alterar permissões de rede virtual privada para uma conta de usuário
 Você pode usar uma rede privada virtual (VPN) para conectar-se ao Windows Server Essentials e acessar todos os recursos que estão armazenados no servidor. Isso é especialmente útil se você tiver um computador cliente que está configurado com contas de rede que podem ser usadas para se conectar a um servidor do Windows Server Essentials hospedado por meio de uma conexão VPN. Todas as contas de usuário recém-criadas no Windows Server Essentials hospedado devem usar VPN para fazer logon no computador cliente pela primeira vez.

##### <a name="to-change-vpn-permissions-for-network-users"></a>Para alterar permissões de VPN para os usuários da rede

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **USUÁRIOS**.

3.  Na lista de contas de usuário, selecione a conta de usuário para a qual você deseja conceder permissões para acessar a área de trabalho remotamente.

4.  No painel ** \> tarefas da conta de usuário<** , clique em **Propriedades**.

5.  Na **<\> Propriedades da conta de usuário**, clique na guia **acesso em qualquer lugar** .

6.  Na guia **Acesso em qualquer lugar**, para permitir que um usuário se conecte ao servidor usando VPN, marque a caixa de seleção **Permitir VPN (rede privada virtual)**.

7.  Clique em **Aplicar** e em **OK**.

###  <a name="change-access-to-internal-shared-folders-for-a-user-account"></a><a name="BKMK_Access12"></a> Alterar o acesso a pastas compartilhadas internas para uma conta de usuário
 Você pode gerenciar o acesso a pastas compartilhadas no servidor usando as tarefas na guia **Pastas do servidor** do Painel. Por padrão, as seguintes pastas do servidor são criadas quando você instala o Windows Server Essentials:

-   **Backups de computador cliente**.  Usada para armazenar backups de computador cliente criados pelo backup do Windows Server. Esta pasta de servidor não é compartilhada.

-   **Empresa**.  Usado para armazenar e acessar documentos relacionados à sua organização pelos usuários da rede.

-   **Backups de histórico de arquivos**.  Por padrão, o Windows Server Essentials armazena backups de arquivos criados usando o histórico de arquivos. Esta pasta de servidor não é compartilhada.

-   **Redirecionamento de pasta**.  Usado para armazenar e acessar pastas que estão configuradas para redirecionamento de pasta pelos usuários da rede. Esta pasta de servidor não é compartilhada.

-   **Música**.  Usado para armazenar e acessar arquivos de música pelos usuários da rede. Essa pasta é criada quando você ativa o compartilhamento de mídia.

-   **Imagens**  Usada para armazenar e acessar imagens por usuários da rede. Essa pasta é criada quando você ativa o compartilhamento de mídia.

-   **TV gravada**.  Usada para armazenar e acessar programas de TV gravados pelos usuários da rede. Essa pasta é criada quando você ativa o compartilhamento de mídia.

-   **Vídeos**.  Usada para armazenar e acessar vídeos pelos usuários da rede. Essa pasta é criada quando você ativa o compartilhamento de mídia.

-   **Usuários**.  Usada para armazenar e acessar arquivos por usuários da rede. Uma pasta específica do usuário é gerada automaticamente na pasta **Usuários** do servidor para cada conta de usuário de rede que você criar.

##### <a name="to-change-access-to-a-shared-folder-for-a-user-account"></a>Para alterar o acesso a uma pasta compartilhada para uma conta de usuário

1.  Abra o Painel do Windows Server Essentials.

2.  Clique em **ARMAZENAMENTO** e clique em **Pastas do servidor**.

3.  Navegue e selecione a pasta do servidor cujas permissões você deseja modificar.

4.  No painel de tarefas, clique em **Exibir as propriedades da pasta**.

5.  Nas ** \> Propriedades<nome_da_pasta**, clique em **compartilhamento**e selecione o nível de acesso do usuário apropriado para as contas de usuário listadas e clique em **aplicar**.

    > [!NOTE]
    >  Não é possível modificar as permissões de compartilhamento das pastas **Backups do histórico de arquivos**, **Redirecionamento de pasta**, e **Usuários** do servidor. Assim, as propriedades dessas pastas de servidor não incluem uma guia **Compartilhamento**.

###  <a name="allow-user-accounts-to-establish-a-remote-desktop-session-to-their-computer"></a><a name="BKMK_Access13"></a> Permitir que contas de usuário estabeleçam uma sessão de área de trabalho remota para seu computador
  Esta seção se aplica a um servidor que executa o Windows Server Essentials ou o Windows Server Essentials ou a um servidor executando o Windows Server 2012 R2 Standard ou o Windows Server 2012 R2 Datacenter com a função de experiência do Windows Server Essentials instalada.

 O administrador de rede pode conceder permissões para usuários de rede, permitindo que eles acessem computadores de sua rede a partir de um local remoto.

##### <a name="to-enable-users-to-access-their-network-computers-from-a-remote-location"></a>Para permitir aos usuários acessar seus computadores da rede a partir de um local remoto

1.  Abra o Painel do Windows Server Essentials.

2.  Na barra de navegação, clique em **USUÁRIOS**.

3.  Na lista de contas de usuário, selecione a conta de usuário a que você deseja conceder permissões para acessar a área de trabalho remotamente.

4.  No painel ** \> tarefas da conta de usuário<** , clique em **Propriedades**.

5.  Na **<\> Propriedades da conta de usuário**, clique na guia acesso do **computador** .

6.  Selecione os computadores que você deseja que a conta de usuário possa acessar remotamente e, em seguida, clique em **OK**.

## <a name="additional-references"></a>Referências adicionais

-   [Gerenciar contas Online para usuários](Manage-Online-Accounts-for-Users.md)

-   [Conecte-se](../use/Get-Connected-in-Windows-Server-Essentials.md)

-   [Utilizar o Windows Server Essentials](../use/Use-Windows-Server-Essentials.md)

-   [Gerenciar o Windows Server Essentials](Manage-Windows-Server-Essentials.md)
