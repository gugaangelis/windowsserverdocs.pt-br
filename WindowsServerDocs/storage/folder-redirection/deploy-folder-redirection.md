---
title: Implantar redirecionamento de pasta com Arquivos Offline
description: Como usar o Windows Server para implantar o redirecionamento de pasta com o Arquivos Offline para computadores cliente Windows.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 21172d9d3e6d91af691986bfd84b0e32049f3b88
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401965"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Implantar redirecionamento de pasta com Arquivos Offline

>Aplica-se a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2, Windows Server (canal semestral)

Este tópico descreve como usar o Windows Server para implantar o redirecionamento de pasta com o Arquivos Offline para computadores cliente Windows.

Para obter uma lista das alterações recentes neste tópico, consulte [histórico de alterações](#change-history).

> [!IMPORTANT]
> Devido às alterações de segurança feitas no [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), [atualizamos a etapa 3: Crie um GPO para o redirecionamento](#step-3-create-a-gpo-for-folder-redirection) de pasta deste tópico para que o Windows possa aplicar corretamente a política de redirecionamento de pasta (e não reverter as pastas redirecionadas nos PCs afetados).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="hardware-requirements"></a>Requisitos de hardware

O redirecionamento de pasta requer um computador baseado em x64 ou x86; Não há suporte para ele no Windows® RT.

### <a name="software-requirements"></a>Requisitos de software

O redirecionamento de pasta tem os seguintes requisitos de software:

- Para administrar o redirecionamento de pasta, você deve estar conectado como um membro do grupo de segurança Administradores de domínio, do grupo de segurança Administradores de empresa ou do grupo de segurança proprietários do Política de Grupo Creator.
- Os computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Os computadores cliente devem ser integrados aos AD DS (Serviços de Domínio Active Directory) que você está gerenciando.
- Um computador deve ser disponibilizado com o Gerenciamento de Política de Grupo e o Centro de Administração do Active Directory instalados.
- Um servidor de arquivos deve estar disponível para hospedar pastas redirecionadas.
    - Se o compartilhamento de arquivos usar os Namespaces do DFS, as pastas DFS (links) deverão ter um único destino para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Se o compartilhamento de arquivos usar a Replicação do DFS para replicar o conteúdo com outro servidor, os usuários devem poder acessar apenas o servidor de origem para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Ao usar um compartilhamento de arquivos clusterizado, desabilite a disponibilidade contínua no compartilhamento de arquivos para evitar problemas de desempenho com redirecionamento de pasta e Arquivos Offline. Além disso, Arquivos Offline pode não passar para o modo offline por 3-6 minutos depois que um usuário perde o acesso a um compartilhamento de arquivos continuamente disponível, o que poderia frustrar os usuários que ainda não estão usando o modo sempre offline do Arquivos Offline.

> [!NOTE]
> Alguns recursos mais recentes do redirecionamento de pasta têm requisitos adicionais de computador cliente e de Active Directory esquema. Para obter mais informações, consulte [implantar computadores primários](deploy-primary-computers.md), [desabilitar arquivos offline em pastas](disable-offline-files-on-folders.md), [habilitar o modo sempre offline](enable-always-offline.md)e [habilitar a movimentação de pastas otimizadas](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Etapa 1: Criar um grupo de segurança de redirecionamento de pasta

Se o seu ambiente ainda não estiver configurado com o redirecionamento de pasta, a primeira etapa será criar um grupo de segurança que contenha todos os usuários aos quais você deseja aplicar as configurações de política de redirecionamento de pasta.

Veja como criar um grupo de segurança para o redirecionamento de pasta:

1. Abra Gerenciador do Servidor em um computador com Active Directory centro de administração instalado.
2. No menu **ferramentas** , selecione **Active Directory centro de administração**. O Centro de Administração do Active Directory é exibido.
3. Clique com o botão direito do mouse no domínio ou unidade organizacional apropriado, selecione **novo**e, em seguida, selecione **grupo**.
4. Na janela **Criar Grupo**, na seção **Grupo**, especifique as seguintes configurações:
    - Em **Nome do grupo**, digite o nome do grupo de segurança, por exemplo: **Usuários de redirecionamento de pasta**.
    - Em **escopo do grupo**, selecione **segurança**e, em seguida, selecione **global**.
5. Na seção **Membros** , selecione **Adicionar**. A caixa de diálogo Selecionar Usuários, Contatos, Computadores, Contas de Serviço ou Grupos é exibida.
6. Digite os nomes dos usuários ou grupos nos quais você deseja implantar o redirecionamento de pasta, selecione **OK**e, em seguida, selecione **OK** novamente.

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Etapa 2: Criar um compartilhamento de arquivos para pastas redirecionadas

Se você ainda não tiver um compartilhamento de arquivos para pastas redirecionadas, use o procedimento a seguir para criar um compartilhamento de arquivos em um servidor que executa o Windows Server 2012.

> [!NOTE]
> Alguma funcionalidade pode ser diferente ou estar indisponível, se você criar o compartilhamento de arquivos em um servidor que executar outra versão do Windows Server.

Veja como criar um compartilhamento de arquivos no Windows Server 2019, no Windows Server 2016 e no Windows Server 2012:

1. No painel de navegação Gerenciador do Servidor, selecione **serviços de arquivo e armazenamento**e, em seguida, selecione **compartilhamentos** para exibir a página compartilhamentos.
2. No bloco **compartilhamentos** , selecione **tarefas**e, em seguida, selecione **novo compartilhamento**. O Assistente Novo Compartilhamento é exibido.
3. Na página **selecionar perfil** , selecione **compartilhamento SMB – rápido**. Se você tiver o Gerenciador de recursos de servidor de arquivos instalado e estiver usando as propriedades de gerenciamento de pastas, selecione **compartilhamento SMB – avançado**.
4. Na página **Compartilhar Local**, selecione o servidor e o volume nos quais deseja criar o compartilhamento.
5. Na página **nome do compartilhamento** , digite um nome para o compartilhamento (por exemplo, **usuários $** ) na caixa **nome do compartilhamento** .
    >[!TIP]
    >Ao criar o compartilhamento, oculte o compartilhamento colocando um ```$``` após o nome do compartilhamento. Isso ocultará o compartilhamento de navegadores casuais.
6. Na página **outras configurações** , desmarque a caixa de seleção Habilitar disponibilidade contínua, se houver, e, opcionalmente, marque as caixas de seleção **habilitar a enumeração baseada em acesso** e **criptografar o acesso a dados** .
7. Na página **permissões** , selecione **Personalizar permissões...** . A caixa de diálogo Configurações de Segurança Avançada é exibida.
8. Selecione **desabilitar herança**e, em seguida, selecione **converter permissões herdadas em permissão explícita neste objeto**.
9. Defina as permissões conforme descrito na tabela 1 e mostrada na Figura 1, removendo permissões para grupos e contas não listadas e adicionando permissões especiais ao grupo de usuários de redirecionamento de pasta que você criou na etapa 1.
    
    ![Definindo as permissões para o compartilhamento de pastas redirecionadas](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)
    
    **Figura 1** Definindo as permissões para o compartilhamento de pastas redirecionadas
10. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Propriedades de Gerenciamento** , selecione o valor de Uso da Pasta de **Arquivos do Usuário** .
11. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Cota** , opcionalmente selecione uma cota para aplicar aos usuários do compartilhamento.
12. Na página **confirmação** , selecione **criar.**

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Permissões necessárias para o compartilhamento de arquivos que hospeda pastas redirecionadas

| Conta de Usuário  | Access  | Aplica-se a  |
| --------- | --------- | --------- |
| Conta de Usuário | Access | Aplica-se a |
| Sistema     | Controle total        |    Essa pasta, subpastas e arquivos     |
| Administradores     | Controle total       | Apenas essa pasta        |
| Criador/Proprietário     |   Controle total      |   Apenas subpastas e arquivos      |
| Grupo de segurança de usuários que precisam colocar dados no compartilhamento (usuários de redirecionamento de pasta)     |   Listar pasta/ler dados *(permissões avançadas)* <br /><br />Criar pastas/acrescentar dados *(permissões avançadas)* <br /><br />Atributos *de leitura (permissões avançadas)* <br /><br />Ler atributos estendidos *(permissões avançadas)* <br /><br />Permissões *de leitura (permissões avançadas)*      |  Apenas essa pasta       |
| Outros grupos e contas     |  Nenhum (remover)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Etapa 3: Criar um GPO para o redirecionamento de pasta

Se você ainda não tiver um GPO criado para as configurações de redirecionamento de pasta, use o procedimento a seguir para criar um.

Veja como criar um GPO para o redirecionamento de pasta:

1. Abra o Gerenciador do Servidor em um computador com o Gerenciamento de Política de Grupo instalado.
2. No menu **ferramentas** , selecione **Gerenciamento de política de grupo**.
3. Clique com o botão direito do mouse no domínio ou na UO em que você deseja configurar o redirecionamento de pasta, selecione **criar um GPO nesse domínio e vincule-o aqui**.
4. Na caixa de diálogo **novo GPO** , digite um nome para o GPO (por exemplo, **configurações de redirecionamento de pasta**) e, em seguida, selecione **OK**.
5. Clique com o botão direito do mouse no GPO recentemente criado e, em seguida, desmarque a caixa de seleção **Vínculo habilitado** . Isso evita que o GPO seja aplicado até que você finalize a configuração dele.
6. Selecione o GPO. Na seção **filtragem de segurança** da guia **escopo** , selecione **usuários autenticados**e, em seguida, selecione **remover** para impedir que o GPO seja aplicado a todos.
7. Na seção **filtragem de segurança** , selecione **Adicionar**.
8. Na caixa de diálogo **Selecionar usuário, computador ou grupo** , digite o nome do grupo de segurança criado na etapa 1 (por exemplo, usuários de **redirecionamento de pasta**) e, em seguida, selecione **OK**.
9. Selecione a guia **delegação** , selecione **Adicionar**, digite **usuários autenticados**, selecione **OK**e, em seguida, selecione **OK** novamente para aceitar as permissões de leitura padrão.
    
    Esta etapa é necessária devido a alterações de segurança feitas em [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

> [!IMPORTANT]
> Devido às alterações de segurança feitas no [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), agora você deve conceder permissões de leitura delegadas ao grupo de usuários autenticados para o GPO de redirecionamento de pasta-caso contrário, o GPO não será aplicado aos usuários ou, se já estiver aplicado, o GPO será removido, redirecionando pastas de volta para o computador local. Para obter mais informações, consulte [Implantando política de grupo atualização de segurança MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Etapa 4: Configurar o redirecionamento de pasta com Arquivos Offline

Depois de criar um GPO para as configurações de redirecionamento de pasta, edite as configurações de Política de Grupo para habilitar e configurar o redirecionamento de pasta, conforme discutido no procedimento a seguir.

> [!NOTE]
> O Arquivos Offline é habilitado por padrão para pastas redirecionadas em computadores cliente do Windows e desabilitado em computadores que executam o Windows Server, a menos que seja alterado pelo usuário. Para usar Política de Grupo para controlar se o Arquivos Offline está habilitado, use a configuração **permitir ou impedir o uso da política de recursos de arquivos offline** .
> Para obter informações sobre algumas das outras Arquivos Offline Política de Grupo configurações, consulte [habilitar a funcionalidade avançada de arquivos offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>)e [configurando política de grupo para arquivos offline](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Veja como configurar o redirecionamento de pasta no Política de Grupo:

1. No gerenciamento de Política de Grupo, clique com o botão direito do mouse no GPO que você criou (por exemplo, **configurações de redirecionamento de pasta**) e selecione **Editar**.
2. Na janela Editor de Gerenciamento de Política de Grupo, navegue até **configuração do usuário**, **diretivas**, configurações do **Windows**e **redirecionamento de pasta**.
3. Clique com o botão direito do mouse em uma pasta que você deseja redirecionar (por exemplo, **documentos**) e selecione **Propriedades**.
4. Na caixa de diálogo **Propriedades** , na caixa **configuração** , selecione **básico – redirecionar a pasta de todos para o mesmo local**.

    > [!NOTE]
    > Para aplicar o redirecionamento de pasta a computadores cliente que executam o Windows XP ou o Windows Server 2003, selecione a guia **configurações** e selecione a **política aplicar redirecionamento para Windows 2000, Windows 2000 Server, Windows XP e Windows Server 2003** caixa de seleção sistemas.

5. Na seção **local da pasta de destino** , selecione **criar uma pasta para cada usuário no caminho raiz** e, em seguida, na caixa **caminho raiz** , digite o caminho para o compartilhamento de arquivos que armazena pastas redirecionadas, por exemplo:  **\\ \\ usuários\\do FS1.Corp.contoso.com $**
6. Selecione a guia **configurações** e, na seção **remoção de política** , opcionalmente, selecione **redirecionar a pasta de volta para o local do USERPROFILE local quando a política for removida** (essa configuração pode ajudar a fazer com que o redirecionamento de pasta se comporte mais previsível para adminisitrators e usuários).
7. Selecione **OK**e, em seguida, selecione **Sim** na caixa de diálogo de aviso.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Etapa 5: Habilitar o GPO de redirecionamento de pasta

Depois de concluir a configuração das configurações de Política de Grupo de redirecionamento de pasta, a próxima etapa é habilitar o GPO, permitindo que ele seja aplicado aos usuários afetados.

> [!TIP]
> Se você planejar implantar suporte de computador primário ou outras configurações de política, faça isso agora, antes de habilitar o GPO. Isso evita que os dados do usuário sejam copiados para computadores não primários antes de o suporte de computador primário ser habilitado.

Veja como habilitar o GPO de redirecionamento de pasta:

1. Abra Gerenciamento de Política de Grupo.
2. Clique com o botão direito do mouse no GPO que você criou e selecione **link habilitado**. Uma caixa de seleção será exibida ao lado do item de menu.

## <a name="step-6-test-folder-redirection"></a>Etapa 6: Testar redirecionamento de pasta

Para testar o redirecionamento de pasta, entre em um computador com uma conta de usuário configurada para o redirecionamento de pasta. Em seguida, confirme se as pastas e os perfis são redirecionados.

Veja como testar o redirecionamento de pasta:

1. Entre em um computador primário (se você tiver habilitado o suporte do computador primário) com uma conta de usuário para a qual você habilitou o redirecionamento de pasta.
2. Caso o usuário tenha sido anteriormente designado para o computador, abra um prompt de comandos com privilégios elevados e, em seguida, digite o comando a seguir para garantir que as configurações de Política de Grupo mais recentes sejam aplicadas ao computador cliente:
    
    ```PowerShell
    gpupdate /force
    ```
3. Abra o Explorador de Arquivos.
4. Clique com o botão direito do mouse em uma pasta redirecionada (por exemplo, a pasta meus documentos na biblioteca de documentos) e selecione **Propriedades**.
5. Selecione a guia **local** e confirme se o caminho exibe o compartilhamento de arquivos especificado, em vez de um caminho local.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Apêndice A: Lista de verificação para implantar o redirecionamento de pasta

| Status           | Ação |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1. Preparar domínio<br>-Ingressar computadores no domínio<br>-Criar contas de usuário |
| ☐<br><br><br>   | 2. Criar grupo de segurança para redirecionamento de pasta<br>-Nome do Grupo:<br>Os |
| ☐<br><br>       | 3. Criar um compartilhamento de arquivos para pastas redirecionadas<br>-Nome do compartilhamento de arquivos: |
| ☐<br><br>       | 4. Criar um GPO para o redirecionamento de pasta<br>-Nome do GPO: |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5. Configurar o redirecionamento de pasta e configurações de política de Arquivos Offline<br>-Pastas redirecionadas:<br>-Suporte do Windows 2000, Windows XP e Windows Server 2003 habilitado?<br>-Arquivos Offline habilitado? (habilitado por padrão em computadores cliente Windows)<br>-O modo sempre offline está habilitado?<br>-A sincronização de arquivo em segundo plano está habilitada?<br>-Movimentação otimizada de pastas redirecionadas habilitada? |
| ☐<br><br>☐<br><br>☐<br>☐ | 6. Adicional Habilitar o suporte ao computador primário<br>-Baseado em computador ou em um usuário?<br>-Designar computadores primários para usuários<br>-Local dos mapeamentos do usuário e do computador primário:<br>-(Opcional) habilitar o suporte ao computador primário para o redirecionamento de pasta<br>-(Opcional) habilitar o suporte a computadores primários para perfis de usuário de roaming |
| ☐         | 7. Habilitar o GPO de redirecionamento de pasta |
| ☐         | 8. Testar redirecionamento de pasta |

## <a name="change-history"></a>Histórico de alterações

A tabela a seguir resume algumas das alterações mais importantes para este tópico.

| Date | Descrição | Reason|
| --- | --- | --- |
| 18 de janeiro de 2017 | Adicionada uma etapa à [etapa 3: Crie um GPO para redirecionamento](#step-3-create-a-gpo-for-folder-redirection) de pasta para delegar permissões de leitura a usuários autenticados, que agora é necessário devido a uma atualização de segurança política de grupo. | Comentários do cliente |

## <a name="more-information"></a>Mais informações

* [Redirecionamento de pasta, Arquivos Offline e perfis de usuário de roaming](folder-redirection-rup-overview.md)
* [Implantar computadores primários para redirecionamento de pasta e perfis de usuário de roaming](deploy-primary-computers.md)
* [Habilitar a funcionalidade de Arquivos Offline avançada](enable-always-offline.md)
* [Declaração de suporte da Microsoft sobre dados de perfil de usuário replicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
* [Aplicativos Sideload com o DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Solução de problemas de empacotamento, implantação e consulta de aplicativos baseados em Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)