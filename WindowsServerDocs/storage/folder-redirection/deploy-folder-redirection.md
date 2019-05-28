---
title: Implantar o redirecionamento de pasta com arquivos Offline
description: Como usar o Windows Server para implantar o redirecionamento de pasta com arquivos Offline em computadores de cliente do Windows.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 07/09/2018
ms.localizationpriority: medium
ms.openlocfilehash: 2bb15d5ae29da6c9dbcd6b58af280026d06febc8
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222742"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Implantar o redirecionamento de pasta com arquivos Offline

>Aplica-se a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2

Este tópico descreve como usar o Windows Server para implantar o redirecionamento de pasta com arquivos Offline em computadores de cliente do Windows.

Para obter uma lista das alterações recentes a este tópico, consulte [histórico de alterações](#change-history).

>[!IMPORTANT]
>Devido às alterações de segurança feitas na [MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), atualizamos [etapa 3: Criar um GPO para redirecionamento de pasta](#step-3-create-a-gpo-for-folder-redirection) deste tópico para que o Windows possa aplicar corretamente a política de redirecionamento de pasta (e não reverter as pastas redirecionadas em computadores afetados).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="hardware-requirements"></a>Requisitos de hardware

Redirecionamento de pasta requer um computador com base em x86 ou x64; não é suportado pelo Windows® retângulo

### <a name="software-requirements"></a>Requisitos de software

Redirecionamento de pasta tem os seguintes requisitos de software:

- Para administrar o redirecionamento de pasta, você deve estar conectado como um membro do grupo de segurança Administradores de domínio, o grupo de segurança Administradores de empresa ou o grupo de segurança de proprietários de criadores de diretiva de grupo.
- Computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Os computadores cliente devem ser integrados aos AD DS (Serviços de Domínio Active Directory) que você está gerenciando.
- Um computador deve ser disponibilizado com o Gerenciamento de Política de Grupo e o Centro de Administração do Active Directory instalados.
- Um servidor de arquivos deve estar disponível para hospedar pastas redirecionadas.
    - Se o compartilhamento de arquivos usar os Namespaces do DFS, as pastas DFS (links) deverão ter um único destino para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Se o compartilhamento de arquivos usar a Replicação do DFS para replicar o conteúdo com outro servidor, os usuários devem poder acessar apenas o servidor de origem para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Ao usar um compartilhamento de arquivos clusterizado, desabilite a disponibilidade contínua no compartilhamento de arquivos para evitar problemas de desempenho com arquivos Offline e redirecionamento de pasta. Além disso, arquivos Offline pode não fazer a transição para o modo offline por 3 a 6 minutos depois que um usuário perder o acesso a um compartilhamento de arquivos continuamente disponíveis, que pode frustrar os usuários que ainda não estiverem usando o modo sempre Offline de arquivos Offline.

>[!NOTE]
>Alguns recursos mais recentes no redirecionamento de pasta têm o computador cliente adicionais e requisitos do esquema do Active Directory. Para obter mais informações, consulte [implantar computadores primários](deploy-primary-computers.md), [desabilitar arquivos Offline em pastas](disable-offline-files-on-folders.md), [habilitar o modo sempre Offline](enable-always-offline.md), e [Enable otimizado mover pasta ](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Etapa 1: Criar um grupo de segurança de redirecionamento de pasta

Se seu ambiente não já está definido com o redirecionamento de pasta, a primeira etapa é criar um grupo de segurança que contém todos os usuários aos quais você deseja aplicar as configurações de diretiva de redirecionamento de pasta.

Aqui está como criar um grupo de segurança para redirecionamento de pasta:

1. Abra o Server Manager em um computador com o Centro de administração do Active Directory instalado.
2. Sobre o **ferramentas** menu, selecione **Centro de administração do Active Directory**. O Centro de Administração do Active Directory é exibido.
3. Clique com botão direito no domínio apropriado ou UO, selecione **New**e, em seguida, selecione **grupo**.
4. Na janela **Criar Grupo**, na seção **Grupo**, especifique as seguintes configurações:
    - Em **Nome do grupo**, digite o nome do grupo de segurança, por exemplo: **Os usuários de redirecionamento de pasta**.
    - Na **escopo do grupo**, selecione **Security**e, em seguida, selecione **Global**.
5. No **membros** seção, selecione **Add**. A caixa de diálogo Selecionar Usuários, Contatos, Computadores, Contas de Serviço ou Grupos é exibida.
6. Digite os nomes dos usuários ou grupos ao qual você deseja implantar o redirecionamento de pasta, selecione **Okey**e, em seguida, selecione **Okey** novamente.

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Etapa 2: Criar um compartilhamento de arquivo para pastas redirecionadas

Se você ainda não tiver um compartilhamento de arquivos para pastas redirecionadas, use o procedimento a seguir para criar um compartilhamento de arquivos em um servidor executando o Windows Server 2012.

>[!NOTE]
>Alguma funcionalidade pode ser diferente ou estar indisponível, se você criar o compartilhamento de arquivos em um servidor que executar outra versão do Windows Server.

Aqui está como criar um compartilhamento de arquivos no Windows Server 2019, Windows Server 2016 e Windows Server 2012:

1. No painel de navegação do Gerenciador do servidor, selecione **serviços de arquivo e armazenamento**e, em seguida, selecione **compartilhamentos** para exibir a página compartilhamentos.
2. No **compartilhamentos** lado a lado, selecione **tarefas**e, em seguida, selecione **novo compartilhamento**. O Assistente Novo Compartilhamento é exibido.
3. Sobre o **Selecionar perfil** , selecione **compartilhamento SMB – rápido**. Se você tiver o Gerenciador de recursos de servidor de arquivos instalado e estiver usando propriedades de gerenciamento de pasta, em vez disso, selecione **compartilhamento SMB - avançado**.
4. Na página **Compartilhar Local**, selecione o servidor e o volume nos quais deseja criar o compartilhamento.
5. No **nome do compartilhamento** página, digite um nome para o compartilhamento de (por exemplo, **usuários$** ) na **nome do compartilhamento** caixa.
    >[!TIP]
    >Ao criar o compartilhamento, oculte o compartilhamento colocando um ```$``` após o nome do compartilhamento. Isso ocultará o compartilhamento de navegadores casuais.
6. Sobre o **outras configurações** página, desmarque a caixa de seleção Habilitar disponibilidade contínua, se estiver presente e, opcionalmente, selecione a **Habilitar enumeração baseada em acesso** e **criptografar o acesso a dados** caixas de seleção.
7. Sobre o **permissões** página, selecione **personalizar permissões...** . A caixa de diálogo Configurações de Segurança Avançada é exibida.
8. Selecione **desabilitar herança**e, em seguida, selecione **converter permissões de herança em permissões explícitas nesse objeto**.
9. Defina as permissões conforme descrito na tabela 1 e mostrado na Figura 1, removendo as permissões para contas e grupos não listados e incluindo permissões especiais para o grupo de usuários de redirecionamento de pasta que você criou na etapa 1.
    
    ![Definir as permissões para o compartilhamento de pastas redirecionadas](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1** definindo as permissões para pastas redirecionadas de compartilhamento
10. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Propriedades de Gerenciamento** , selecione o valor de Uso da Pasta de **Arquivos do Usuário** .
11. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Cota** , opcionalmente selecione uma cota para aplicar aos usuários do compartilhamento.
12. Sobre o **confirmação** página, selecione **criar.**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>As permissões necessárias para o arquivo compartilham pastas redirecionadas de hospedagem


|Conta de Usuário  |Acesso  |Aplica-se a  |
|---------|---------|---------|
| Conta de Usuário | Acesso | Aplica-se a |
|Sistema     | Controle total        |    Essa pasta, subpastas e arquivos     |
|Administradores     | Controle total       | Apenas essa pasta        |
|Criador/Proprietário     |   Controle total      |   Apenas subpastas e arquivos      |
|Grupo de segurança de usuários que precisam colocar dados no compartilhamento (usuários de redirecionamento de pasta)     |   Listar pastas / ler dados *(permissões avançadas)* <br /><br />Criar pastas / anexar dados *(permissões avançadas)* <br /><br />Ler atributos *(permissões avançadas)* <br /><br />Ler atributos estendidos *(permissões avançadas)* <br /><br />Permissões de leitura *(permissões avançadas)*      |  Apenas essa pasta       |
|Outros grupos e contas     |  Nenhum (remover)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Etapa 3: Criar um GPO para redirecionamento de pasta

Se você ainda não tiver um GPO criado para as configurações de redirecionamento de pasta, use o procedimento a seguir para criar um.

Aqui está como criar um GPO para redirecionamento de pasta:

1. Abra o Gerenciador do Servidor em um computador com o Gerenciamento de Política de Grupo instalado.
2. Dos **ferramentas** menu, selecione **Group Policy Management**.
3. Clique com botão direito no domínio ou UO em que você deseja configurar o redirecionamento de pasta, em seguida, selecione **criar um GPO neste domínio e vinculá-lo aqui**.
4. No **novo GPO** caixa de diálogo, digite um nome para o GPO (por exemplo, **configurações de redirecionamento de pasta**) e, em seguida, selecione **Okey**.
5. Clique com o botão direito do mouse no GPO recentemente criado e, em seguida, desmarque a caixa de seleção **Vínculo habilitado** . Isso evita que o GPO seja aplicado até que você finalize a configuração dele.
6. Selecione o GPO. No **filtragem de segurança** seção o **escopo** guia, selecione **Authenticated Users**e, em seguida, selecione **remover** para impedir que o GPO seja aplicada a todos.
7. No **filtragem de segurança** seção, selecione **Add**.
8. No **Selecionar usuário, computador ou grupo** caixa de diálogo, digite o nome da segurança do grupo que você criou na etapa 1 (por exemplo, **usuários de redirecionamento de pasta**) e, em seguida, selecione **Okey**.
9. Selecione o **delegação** guia, selecione **Add**, digite **Authenticated Users**, selecione **Okey**e, em seguida, selecione **Okey** novamente aceitar o padrão permissões de leitura.
    
    Essa etapa é necessária devido a alterações de segurança feitas na [MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

>[!IMPORTANT]
>Devido às alterações de segurança feitas na [MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), agora você deve fornecer permissões de leitura de grupo delegado a usuários autenticados para o GPO de redirecionamento de pasta – caso contrário, o GPO não será aplicado a usuários ou se ele já for aplicado, o GPO é removido, o redirecionamento de pastas para o computador local. Para obter mais informações, consulte [implantação de grupo de política de segurança atualização MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Etapa 4: Configurar o redirecionamento de pasta com arquivos Offline

Depois de criar um GPO para as configurações de redirecionamento de pasta, edite as configurações de diretiva de grupo para habilitar e configurar o redirecionamento de pasta, conforme discutido no procedimento a seguir.

>[!NOTE]
>Arquivos offline é habilitado por padrão para pastas redirecionadas em computadores de cliente do Windows e desabilitado em computadores que executam o Windows Server, a menos que alteradas pelo usuário. Para usar política de grupo para controlar se os arquivos Offline é habilitado, use o **permitir ou impedir o uso do recurso Arquivos Offline** configuração de política.
> Para obter informações sobre algumas das configurações de diretiva de grupo de arquivos Offline, consulte [habilitar avançadas Offline arquivos funcionalidade](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>), e [Configurando a diretiva de grupo para arquivos Offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Aqui está como configurar o redirecionamento de pasta na política de grupo:

1. Em gerenciamento de diretiva de grupo, clique com botão direito no GPO criado por você (por exemplo, **configurações de redirecionamento de pasta**) e, em seguida, selecione **editar**.
2. Na janela do Editor de gerenciamento de diretiva de grupo, navegue até **configuração do usuário**, em seguida, **diretivas**, em seguida, **as configurações do Windows**e, em seguida, **pasta Redirecionamento**.
3. Clique em uma pasta que você deseja redirecionar (por exemplo, **documentos**) e, em seguida, selecione **propriedades**.
4. No **propriedades** caixa de diálogo, da **configuração** caixa, selecione **básico - redirecionamento de pasta de todos no mesmo local**.

    > [!NOTE]
    > Para aplicar o redirecionamento de pasta para computadores cliente que executam o Windows XP ou Windows Server 2003, selecione a **as configurações** e selecione o **também aplicar a diretiva de redirecionamento para o Windows 2000, Windows 2000 Server, Windows XP, e Sistemas operacionais Windows Server 2003** caixa de seleção.
5. No **local da pasta de destino** seção, selecione **criar uma pasta para cada usuário no caminho raiz** e, em seguida, no **caminho raiz** , digite o caminho para o armazenamento de compartilhamento de arquivo pastas redirecionadas, por exemplo:  **\\ \\fs1.corp.contoso.com\\$ de usuários**
6. Selecione o **as configurações** guia e, na **política de remoção** seção, selecione opcionalmente **redirecionar a pasta de volta para o perfil de usuário local quando a política for removida** (essa configuração pode ajudar a tornar o redirecionamento de pasta se comportam de maneira mais previsível para adminisitrators e usuários).
7. Selecione **Okey**e, em seguida, selecione **Sim** na caixa de diálogo de aviso.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Etapa 5: Habilitar o GPO de redirecionamento de pasta

Depois que você terminar de definir as configurações de política de grupo de redirecionamento de pasta, a próxima etapa é habilitar o GPO, permitindo que ele seja aplicado aos usuários afetados.

>[!TIP]
>Se você planejar implantar suporte de computador primário ou outras configurações de política, faça isso agora, antes de habilitar o GPO. Isso evita que os dados do usuário sejam copiados para computadores não primários antes de o suporte de computador primário ser habilitado.

Aqui está como habilitar o GPO de redirecionamento de pasta:

1. Abra Gerenciamento de Política de Grupo.
2. Clique com botão direito no GPO que você criou e, em seguida, selecione **vínculo habilitado**. Uma caixa de seleção aparecerá ao lado do item de menu.

## <a name="step-6-test-folder-redirection"></a>Etapa 6: Redirecionamento de pasta de teste

Para testar o redirecionamento de pasta, entre em um computador com uma conta de usuário configurada para redirecionamento de pasta. Em seguida, confirme que as pastas e os perfis são redirecionados.

Aqui está como testar o redirecionamento de pasta:

1. Entrar para um computador primário (se você habilitou o suporte de computador primário) com uma conta de usuário para o qual você habilitou o redirecionamento de pasta.
2. Caso o usuário tenha sido anteriormente designado para o computador, abra um prompt de comandos com privilégios elevados e, em seguida, digite o comando a seguir para garantir que as configurações de Política de Grupo mais recentes sejam aplicadas ao computador cliente:
    
    ```PowerShell
    gpupdate /force
    ```
3. Abra o Explorador de Arquivos.
4. Clique em uma pasta redirecionada (por exemplo, a pasta Meus documentos da biblioteca de documentos) e, em seguida, selecione **propriedades**.
5. Selecione o **local** guia e confirme se o caminho de compartilhamento de arquivos especificado, em vez de um caminho local é exibida.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Apêndice a: Lista de verificação para implantação de redirecionamento de pasta

|Status|Ação|
|:---:|---|
|☐<br>☐<br>☐|1. Preparar domínio<br>-Ingressar computadores ao domínio<br>-Criar contas de usuário|
|☐<br><br><br>|2. Criar grupo de segurança para redirecionamento de pasta<br>-Nome do grupo de:<br>-Membros:|
|☐<br><br>|3. Criar um compartilhamento de arquivo para pastas redirecionadas<br>-Nome do compartilhamento de arquivos:|
|☐<br><br>|4. Criar um GPO para redirecionamento de pasta<br>-Nome do GPO:|
|☐<br><br>☐<br>☐<br>☐<br>☐<br>☐|5. Definir as configurações de política de redirecionamento de pasta e arquivos Offline<br>-Redirecionamento de pastas:<br>-Windows 2000, Windows XP e Windows Server 2003 suporte habilitado?<br>-Arquivos off-line habilitados? (habilitado por padrão em computadores de cliente do Windows)<br>-Modo off-line sempre habilitado?<br>-Sincronização de arquivos de plano de fundo habilitada?<br>-Move otimizado de pastas redirecionadas habilitado?|
|☐<br><br>☐<br><br>☐<br>☐|6. (Opcional) Habilitar o suporte de computador primário<br>-Baseadas em computador ou com base no usuário?<br>-Designar computadores primários para usuários<br>-Local do usuário e os mapeamentos de computador primário:<br>-(Opcional) Habilitar suporte a computadores primários para redirecionamento de pasta<br>-(Opcional) Habilitar suporte de computador primário para perfis de usuário móvel|
|☐|7. Habilitar o GPO de redirecionamento de pasta|
|☐|8. Redirecionamento de pasta de teste|

## <a name="change-history"></a>Histórico de alterações

A tabela a seguir resume algumas das alterações mais importantes para este tópico.

|Date|Descrição|Reason|
|---|---|---|
|18 de janeiro de 2017|Adicionada uma etapa para [etapa 3: Criar um GPO para redirecionamento de pasta](#step-3-create-a-gpo-for-folder-redirection) delegar permissões de leitura para usuários autenticados, que agora é necessária devido a uma atualização de segurança de diretiva de grupo.|Comentários do cliente.|

## <a name="more-information"></a>Mais informações

* [Redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](folder-redirection-rup-overview.md)
* [Implantar computadores primários para redirecionamento de pasta e perfis de usuário móvel](deploy-primary-computers.md)
* [Habilitar a funcionalidade avançada de arquivos Offline](enable-always-offline.md)
* [Instrução de suporte da Microsoft em torno de dados de perfil de usuário duplicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Sideload de aplicativos com o DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Solução de problemas de empacotamento, implantação e consulta de aplicativos baseados em tempo de execução do Windows](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)