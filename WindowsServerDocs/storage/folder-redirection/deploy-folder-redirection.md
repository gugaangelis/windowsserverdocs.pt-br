---
title: Implanta o redirecionamento de pasta com arquivos Offline
description: Como usar o Windows Server para implantar o redirecionamento de pasta com arquivos Offline em computadores de cliente do Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 33942db34314e0ff60b24d4b9c8e5e33b4ca92fd
ms.sourcegitcommit: 5549ac178f8f3d116e88761a95223063a636ac94
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/04/2018
ms.locfileid: "4376616"
---
# Implanta o redirecionamento de pasta com arquivos Offline

>Aplica-se a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Server 2008 R2, Windows Server 2012, Windows Server 2012 R2, Windows Server 2016, Windows Vista

Este tópico descreve como usar o Windows Server para implantar o redirecionamento de pasta com os arquivos Offline para computadores de cliente do Windows.

Para obter uma lista de alterações recentes para este tópico, consulte o [histórico de alterações](#change-history).

>[!IMPORTANT]
>Devido às alterações de segurança feitas no [MS16-072](https://support.microsoft.com/en-us/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), atualizamos [etapa 3: criar um GPO para redirecionamento de pasta](#step-3:-create-a-gpo-for-folder-redirection) deste tópico para que o Windows possa corretamente, aplique a política de redirecionamento de pasta (e não reverter pastas redirecionadas em computadores afetados).

## Pré-requisitos

### Requisitos de hardware

Redirecionamento de pasta requer um computador baseado em x86 ou x64. ele não é compatível com Windows® retângulo

### Requisitos de software

Redirecionamento de pasta tem os seguintes requisitos de software:

- Para administrar o redirecionamento de pasta, você deve estar conectado como um membro do grupo de segurança Administradores de domínio, o grupo de segurança Administradores de empresa ou o grupo de segurança proprietários de criadores de diretiva de grupo.
- Os computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Os computadores cliente devem ser ingressados no Active Directory Domain Services (AD DS) que você está gerenciando.
- Um computador deve estar disponível com gerenciamento de política de grupo e o Centro de administração do Active Directory instalado.
- Um servidor de arquivos deve estar disponível para hospedar pastas redirecionadas.
    - Se o compartilhamento de arquivos usa Namespaces DFS, as pastas DFS (links) devem ter um único destino para impedir que os usuários façam alterações conflitantes em servidores diferentes.
    - Se o compartilhamento de arquivos usa a replicação DFS para replicar o conteúdo com outro servidor, os usuários devem ser capazes de acessar somente o servidor de origem para impedir que os usuários façam alterações conflitantes em servidores diferentes.
    - Ao usar um compartilhamento de arquivos em cluster, desabilite a disponibilidade contínua no compartilhamento de arquivos para evitar problemas de desempenho com redirecionamento de pasta e arquivos Offline. Além disso, arquivos Offline pode não fazer a transição para o modo offline por 3 a 6 minutos depois que um usuário perde o acesso a um compartilhamento de arquivos continuamente disponíveis que poderia frustrante usuários que ainda não estiverem usando o modo sempre Offline dos arquivos Offline.

>[!NOTE]
>Alguns recursos mais recentes do redirecionamento de pasta têm o computador cliente adicionais e requisitos de esquema do Active Directory. Para obter mais informações, consulte [implantar principais computadores](deploy-primary-computers.md), [Desabilitar Offline arquivos em pastas](disable-offline-files-on-folders.md), [Habilitar sempre modo Offline](enable-always-offline.md)e [permite a movimentação de pasta otimizada](enable-optimized-moving.md).

## Etapa 1: Criar um grupo de segurança de redirecionamento de pasta

Se seu ambiente não é já configurado com redirecionamento de pasta, a primeira etapa é criar um grupo de segurança que contém todos os usuários para o qual você deseja aplicar configurações de política de redirecionamento de pasta.

Aqui está como criar um grupo de segurança para o redirecionamento de pasta:

1. Abrir o Gerenciador de servidor em um computador com o Centro de administração do Active Directory instalado.
2. No menu **Ferramentas** , selecione o **Centro de administração do Active Directory**. O Centro de Administração do Active Directory é exibido.
3. Clique com botão direito no domínio apropriado ou unidade Organizacional, selecione o **novo**e, em seguida, selecione o **grupo**.
4. Na janela **Criar Grupo**, na seção **Grupo**, especifique as seguintes configurações:
    - Em **nome do grupo**, digite o nome do grupo de segurança, por exemplo: **Os usuários de redirecionamento de pasta**.
    - No **escopo do grupo**, selecione a **segurança**e, em seguida, selecione **Global**.
5. Na seção **membros** , selecione **Adicionar**. A caixa de diálogo Selecionar Usuários, Contatos, Computadores, Contas de Serviço ou Grupos é exibida.
6. Digite os nomes dos usuários ou grupos aos quais você deseja implantar o redirecionamento de pasta, selecione **Okey**e, em seguida, selecione **Okey** novamente.

## Etapa 2: Criar um compartilhamento de arquivos para pastas redirecionadas

Se você não tiver um compartilhamento de arquivos para pastas redirecionadas, use o procedimento a seguir para criar um compartilhamento de arquivos em um servidor que executa o Windows Server 2012.

>[!NOTE]
>Algumas funcionalidades podem diferir ou não estar disponível se você criar o compartilhamento de arquivos em um servidor que executa outra versão do Windows Server.

Aqui está como criar um compartilhamento de arquivos no Windows Server 2012 e Windows Server 2016:

1. No painel de navegação do Gerenciador do servidor, selecione o **arquivo e serviços de armazenamento**e selecione **compartilhamentos** para exibir a página de compartilhamentos.
2. No bloco do **compartilhamentos** , selecione **tarefas**e, em seguida, selecione o **Novo compartilhamento**. O Assistente de novo compartilhamento é exibido.
3. Na página **Selecione perfil** , selecione o **Compartilhamento SMB – rápida**. Se você tiver o Gerenciador de recursos de servidor de arquivos instalado e estiver usando as propriedades de gerenciamento de pasta, em vez disso, selecione o **Compartilhamento SMB - avançada**.
4. Na página **Local compartilhar** , selecione o servidor e o volume no qual você deseja criar o compartilhamento.
5. Na página **Nome do compartilhamento** , digite um nome para o compartilhamento (por exemplo, **os usuários$**) na caixa de **nome do compartilhamento** .
    >[!TIP]
    >Ao criar o compartilhamento, ocultar o compartilhamento colocando um ```$``` após o nome do compartilhamento. Isso ocultará o compartilhamento de navegadores casuais.
6. Na página **Outras configurações** , desmarque a caixa de seleção Habilitar disponibilidade contínua, se presente e, opcionalmente, marque as caixas de seleção de **Habilitar a enumeração baseada em acesso** e **acesso a criptografar dados** .
7. Na página **permissões** , selecione **Personalizar permissões...**. A caixa de diálogo de configurações de segurança avançadas é exibida.
8. Selecione **Desabilitar a herança**e, em seguida, selecione **Converter herdado permissões em permissão explícita nesse objeto**.
9. Defina as permissões como descrito tabela 1 e mostrado na Figura 1, removendo permissões para contas e grupos não listados e adicione as permissões especiais para o grupo de usuários de redirecionamento de pasta que você criou na etapa 1.
    
    ![Definindo as permissões para o compartilhamento de pastas redirecionadas](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1** Definindo as permissões para o compartilhamento de pastas redirecionadas
10. Se você escolheu o perfil de **Compartilhamento SMB - avançada** , na página de **Propriedades de gerenciamento** , selecione o valor de uso da pasta de **Arquivos do usuário** .
11. Se você escolheu o perfil de **Compartilhamento SMB - avançada** , na página de **cota** , opcionalmente, selecione uma cota sejam aplicadas a usuários do compartilhamento.
12. Na página de **confirmação** , selecione **Create.**

### Permissões necessárias para o arquivo compartilham hospedagem pastas redirecionadas

<table>
<tbody>
<tr class="odd">
<td>Conta de usuário</td>
<td>Access</td>
<td>Aplicável a</td>
</tr>
<tr class="even">
<td>Sistema</td>
<td>Controle total</td>
<td>Esta pasta, subpastas e arquivos</td>
</tr>
<tr class="odd">
<td>Administradores</td>
<td>Controle total</td>
<td>Esta pasta somente</td>
</tr>
<tr class="even">
<td>Proprietário/criador</td>
<td>Controle total</td>
<td>Somente arquivos e subpastas</td>
</tr>
<tr class="odd">
<td>Grupo de segurança de usuários que precisam inserir dados no compartilhamento (usuários de redirecionamento de pasta)</td>
<td>Listar pasta / ler dados<sup>1</sup><br />
<br />
Criar pastas / acrescentar dados<sup>1</sup><br />
<br />
Ler os atributos<sup>1</sup><br />
<br />
Atributos estendidos de leitura<sup>1</sup><br />
<br />
Permissões de leitura<sup>1</sup></td>
<td>Esta pasta somente</td>
</tr>
<tr class="even">
<td>Outras contas e grupos</td>
<td>Nenhum (remover)</td>
<td></td>
</tr>
</tbody>
</table>

1 permissões avançadas

## Etapa 3: Criar um GPO para redirecionamento de pasta

Se você não tiver um GPO criado para configurações de redirecionamento de pasta, use o procedimento a seguir para criar um.

Aqui está como criar um GPO para redirecionamento de pasta:

1. Abrir o Gerenciador de servidor em um computador com o gerenciamento de política de grupo instalado.
2. No menu **Ferramentas** , selecione o **Gerenciamento de política de grupo**.
3. Clique com botão direito no domínio ou unidade Organizacional em que você deseja configurar o redirecionamento de pasta e selecione **criar um GPO neste domínio e vinculá-lo aqui**.
4. Na caixa de diálogo **Novo GPO** , digite um nome para o GPO (por exemplo, **As configurações de redirecionamento de pasta**) e, em seguida, selecione **Okey**.
5. Clique com botão direito no GPO recém-criado e, em seguida, desmarque a caixa de seleção **Vínculo habilitado** . Isso impede que o GPO sejam aplicadas até que você terminar de configurá-lo.
6. Selecione o GPO. Na seção **Filtragem de segurança** da guia **escopo** , selecione **Usuários autenticados**e, em seguida, selecione **Remover** para impedir que o GPO que está sendo aplicado a todos os usuários.
7. Na seção **Filtragem de segurança** , selecione **Adicionar**.
8. Na caixa de diálogo **Selecionar usuário, computador ou grupo** , digite o nome do grupo de segurança que você criou na etapa 1 (por exemplo, **Os usuários de redirecionamento de pasta**) e, em seguida, selecione **Okey**.
9. Selecione a guia de **delegação** , selecione **Add**, digite **Usuários autenticados**, selecione **Okey**e, em seguida, selecione **Okey** novamente para aceitar as permissões de leitura padrão.
    
    Esta etapa é necessária devido a alterações de segurança feitas no [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

>[!IMPORTANT]
>Devido a segurança as alterações feitas no [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), agora você devem fornecer o grupo de usuários autenticados leitura permissões delegadas para o GPO de redirecionamento de pasta - caso contrário, o GPO não é aplicado aos usuários ou se tiver sido aplicado, o GPO é removido, redirecionando pastas de volta para o computador local. Para obter mais informações, consulte [Implantando grupo política de segurança da atualização MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## Etapa 4: Configurar o redirecionamento de pasta com arquivos Offline

Depois de criar um GPO para configurações de redirecionamento de pasta, edite as configurações de política de grupo para habilitar e configurar o redirecionamento de pasta, conforme discutido no procedimento a seguir.

>[!NOTE]
>Arquivos offline é habilitado por padrão para pastas redirecionadas em computadores de cliente do Windows e desabilitada em computadores que executam o Windows Server, a menos que alterado pelo usuário. Para usar a política de grupo para controlar se os arquivos Offline está habilitado, use o **Permitir ou impedir o uso do recurso Arquivos Offline** configuração de política.
> Para obter informações sobre algumas das configurações de política de grupo de arquivos Offline, consulte [Habilitar avançada Offline arquivos funcionalidade](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)e [Configurando a política de grupo para arquivos Offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Veja como configurar o redirecionamento de pasta na política de grupo:

1. No gerenciamento de política de grupo, clique com botão direito no GPO que você criou (por exemplo, **As configurações de redirecionamento de pasta**) e, em seguida, selecione **Editar**.
2. Na janela do Editor de gerenciamento de política de grupo, navegue até **Configuração do usuário**, em seguida, **políticas**, em seguida, **Configurações do Windows**e, em seguida, **Redirecionamento de pasta**.
3. Clique com botão direito uma pasta que você deseja redirecionar (por exemplo, **documentos**) e, em seguida, selecione **Propriedades**.
4. Na caixa de diálogo **Propriedades** , na caixa **configuração** , selecione **Básico - Redireciona pastas de todos os usuários para o mesmo local**.

    > [!NOTE]
    > Para aplicar o redirecionamento de pasta para computadores cliente que executam o Windows XP ou Windows Server 2003, selecione a guia **configurações** e selecione o **também aplicar diretiva de redirecionamento para o Windows 2000, Windows 2000 Server, Windows XP e Windows Server 2003 operando sistemas** caixa de seleção.
5. Na seção **local da pasta de destino** , selecione **criar uma pasta para cada usuário no caminho raiz** e, em seguida, na caixa **Caminho raiz** , digite o caminho para o compartilhamento de arquivos armazenando pastas redirecionadas, por exemplo: \\\fs1.corp.contoso.com\\ ** os usuários$**
6. Selecione a guia **configurações** e, na seção **Remoção de política** , selecione opcionalmente **redirecionar a pasta de volta para o perfil de usuário local quando a política é removida** (essa configuração pode ajudar a tornar o redirecionamento de pasta se comportam mais previsível para usuários e adminisitrators).
7. Selecione **Okey**e, em seguida, selecione **Sim** na caixa de diálogo de aviso.

## Etapa 5: Habilitar o redirecionamento de pasta GPO

Depois que você concluiu a definir as configurações de política de grupo de redirecionamento de pasta, a próxima etapa é habilitar o GPO, permitindo que ele seja aplicado aos usuários afetados.

>[!TIP]
>Se você pretende implementar o suporte de computador principal ou outras configurações de política, faça isso agora, antes de habilitar o GPO. Isso impede que os dados do usuário sejam copiadas para computadores não principal antes de suporte de computador principal está habilitada.

Veja como habilitar o GPO de redirecionamento de pasta:

1. Gerenciamento de política de grupo de abrir.
2. Clique com botão direito no GPO que você criou e, em seguida, selecione o **Link habilitado**. Aparecerá uma caixa de seleção ao lado do item de menu.

## Etapa 6: Testar o redirecionamento de pasta

Para testar o redirecionamento de pasta, entrar em um computador com uma conta de usuário configurada para o redirecionamento de pasta. Em seguida, confirme que as pastas e os perfis são redirecionados.

Aqui está como testar o redirecionamento de pasta:

1. Entrar um computador principal (se habilitado o suporte de computador primário) com uma conta de usuário para o qual você habilitou o redirecionamento de pasta.
2. Se o usuário tem previamente conectado ao computador, abra um prompt de comando com privilégios elevados e digite o seguinte comando para garantir que as configurações de política de grupo mais recentes sejam aplicadas ao computador cliente:
    
    ```PowerShell
    gpupdate /force
    ```
3. Abra o Explorador de arquivos.
4. Clique com botão direito uma pasta redirecionada (por exemplo, a pasta de documentos na biblioteca documentos) e, em seguida, selecione **Propriedades**.
5. Selecione a guia **local** e confirme se o caminho exibe o compartilhamento de arquivos que você especificou em vez de um caminho local.

## Apêndice a: lista de verificação para a implantação de redirecionamento de pasta

|Status|Ação|
|:---:|---|
|☐<br>☐<br>☐|1. preparar o domínio<br>-Adicionar computadores ao domínio<br>-Criar contas de usuário|
|☐<br><br><br>|2. criar o grupo de segurança para o redirecionamento de pasta<br>-Nome do grupo:<br>-Membros:|
|☐<br><br>|3. criar um compartilhamento de arquivos para pastas redirecionadas<br>-Nome do compartilhamento de arquivo:|
|☐<br><br>|4. criar um GPO para redirecionamento de pasta<br>-Nome do GPO:|
|☐<br><br>☐<br>☐<br>☐<br>☐<br>☐|5. definir configurações de política de redirecionamento de pasta e arquivos Offline<br>-Pastas redirecionadas:<br>-Windows 2000, Windows XP e suporte do Windows Server 2003 habilitado?<br>-Arquivos offline habilitados? (ativada por padrão em computadores de cliente do Windows)<br>-Modo Offline sempre ativado?<br>-Sincronização de arquivos em segundo plano habilitada?<br>-Otimizado movimento das pastas redirecionadas habilitado?|
|☐<br><br>☐<br><br>☐<br>☐|6. (suporte a computadores principal habilitar opcional)<br>-Baseado no computador ou com base em usuário?<br>-Designar computadores principais para usuários<br>-Local do usuário e os mapeamentos de computador principal:<br>-Ativa (opcional) o suporte do computador principal para o redirecionamento de pasta<br>-Ativa (opcional) o suporte de computador primário para perfis de usuário móvel|
|☐|7. habilitar o redirecionamento de pasta GPO|
|☐|8. redirecionamento de pasta de teste|

## Histórico de alterações

A tabela a seguir resume algumas das mudanças mais importantes para este tópico.

|Data|Descrição|Motivo|
|---|---|---|
|18 de janeiro de 2017|Adicionada uma etapa para [etapa 3: criar um GPO para redirecionamento de pasta](#step-3:-create-a-gpo-for-folder-redirection) delegar permissões de leitura para usuários autenticados, que agora é necessário devido a uma atualização de segurança de política de grupo.|Comentários do cliente.|

## Mais informações

* [Visão geral de Redirecionamento de Pasta, Arquivos Offline e perfis de usuários móveis](folder-redirection-rup-overview.md)
* [Implantar computadores principais para redirecionamento de pasta e perfis de usuário móvel](deploy-primary-computers.md)
* [Habilitar a funcionalidade avançada Arquivos Offline](enable-always-offline.md)
* [Instrução de suporte da Microsoft em torno de dados de perfil de usuário replicado](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Sideload de aplicativos com o DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Solução de problemas de empacotamento, implantação e consulta de aplicativos com base em tempo de execução do Windows](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)