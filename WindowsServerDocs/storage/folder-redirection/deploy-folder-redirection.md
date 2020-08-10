---
title: Implantar o Redirecionamento de Pastas com o Redirecionamento de Pastas FilesDeploy Offline com Arquivos Offline
description: Como usar o Windows Server para implantar o Redirecionamento de Pastas com o Arquivos Offline para computadores cliente Windows.
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 368feec41fbecee57eb82bbe54a6fe31a9d0e3b5
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954703"
---
# <a name="deploy-folder-redirection-with-offline-files"></a>Implantar Redirecionamento de Pastas com Arquivos Offline

> Aplica-se a: Windows 10, Windows 7, Windows 8, Windows 8.1, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2, Windows Server 2008 R2, Windows Server (Canal Semestral)

Este tópico descreve como usar o Windows Server para implantar o Redirecionamento de Pastas com o Arquivos Offline para computadores cliente Windows.

Para obter uma lista das alterações recentes a este tópico, confira o [Histórico de alterações](#change-history).

> [!IMPORTANT]
> Devido às alterações de segurança feitas no [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), atualizamos a [Etapa 3: Criar um GPO para Redirecionamento de Pastas](#step-3-create-a-gpo-for-folder-redirection) deste tópico para que o Windows possa aplicar corretamente a política de Redirecionamento de Pastas (e não reverter as pastas redirecionadas nos PCs afetados).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="hardware-requirements"></a>Requisitos de hardware

O Redirecionamento de Pastas requer um computador baseado em x64 ou x86; não há suporte para ele no Windows&reg; RT.

### <a name="software-requirements"></a>Requisitos de software

O Redirecionamento de Pastas tem os seguintes requisitos de software:

- Para administrar o Redirecionamento de Pastas, você deve estar conectado como um membro do grupo de segurança Administradores de Domínio, o grupo de segurança Administradores Corporativos ou o grupo de segurança Proprietários Criadores de Política de Grupo.
- Computadores cliente devem executar o Windows 10, o Windows 8.1, o Windows 8, o Windows 7, o Windows Server 2019, o Windows Server 2016, o Windows Server (Canal Semestral), o Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2008 R2 ou o Windows Server 2008.
- Os computadores cliente devem ser integrados aos AD DS (Serviços de Domínio Active Directory) que você está gerenciando.
- Um computador deve ser disponibilizado com o Gerenciamento de Política de Grupo e o Centro de Administração do Active Directory instalados.
- Um servidor de arquivos deve estar disponível para hospedar pastas redirecionadas.
    - Se o compartilhamento de arquivos usar os Namespaces do DFS, as pastas DFS (links) deverão ter um único destino para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Se o compartilhamento de arquivos usar a Replicação do DFS para replicar o conteúdo com outro servidor, os usuários devem poder acessar apenas o servidor de origem para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Ao usar um compartilhamento de arquivo clusterizado, desabilite a disponibilidade contínua no compartilhamento de arquivo para evitar problemas de desempenho com Redirecionamento de Pastas e Arquivos Offline. Além disso, os Arquivos Offline podem não fazer a transição para o modo offline por 3 a 6 minutos depois que um usuário perde o acesso a um compartilhamento de arquivo continuamente disponível, o que pode frustrar os usuários que ainda não estão usando o modo Sempre Offline de Arquivos Offline.

> [!NOTE]
> Alguns recursos mais recentes no Redirecionamento de Pastas têm requisitos adicionais de computador cliente e esquema do Active Directory. Para obter mais informações, confira [Implantar computadores primários](deploy-primary-computers.md), [Desabilitar Arquivos Offline em pastas](disable-offline-files-on-folders.md), [Habilitar o modo sempre offline](enable-always-offline.md) e [Habilitar a movimentação de pastas otimizada](enable-optimized-moving.md).

## <a name="step-1-create-a-folder-redirection-security-group"></a>Etapa 1: Criar um grupo de segurança de Redirecionamento de Pastas

Se o ambiente ainda não estiver configurado com o Redirecionamento de Pastas, a primeira etapa será criar um grupo de segurança que contenha todos os usuários aos quais você deseja aplicar as configurações de política de Redirecionamento de Pastas.

Veja como criar um grupo de segurança para o Redirecionamento de Pastas:

1. Abra o Gerenciador do Servidor em um computador com o Centro de Administração do Active Directory instalado.
2. No menu **Ferramentas**, selecione **Centro de Administração do Active Directory**. O Centro de Administração do Active Directory é exibido.
3. Clique com o botão direito do mouse no domínio ou unidade organizacional apropriado, selecione **Novo** e, em seguida, selecione **Grupo**.
4. Na janela **Criar Grupo** , na seção **Grupo** , especifique as seguintes configurações:
    - Em **Nome do grupo**, digite o nome do grupo de segurança, por exemplo: **Usuários do Redirecionamento de Pastas**.
    - Em **Escopo do grupo**, selecione **Segurança** e, em seguida, selecione **Global**.
5. Na seção **Membros**, selecione **Adicionar**. A caixa de diálogo Selecionar Usuários, Contatos, Computadores, Contas de Serviço ou Grupos é exibida.
6. Digite os nomes dos usuários ou grupos nos quais você deseja implantar o Redirecionamento de Pastas, selecione **OK** e, em seguida, selecione **OK** novamente.

## <a name="step-2-create-a-file-share-for-redirected-folders"></a>Etapa 2: Criar um compartilhamento de arquivo para pastas redirecionadas

Se você ainda não tiver um compartilhamento de arquivo para pastas redirecionadas, use o procedimento a seguir para criar um compartilhamento de arquivo em um servidor que executa o Windows Server 2012.

> [!NOTE]
> Alguma funcionalidade pode ser diferente ou estar indisponível, se você criar o compartilhamento de arquivos em um servidor que executar outra versão do Windows Server.

Veja como criar um compartilhamento de arquivo no Windows Server 2019, no Windows Server 2016 e no Windows Server 2012:

1. No painel de navegação Gerenciador do Servidor, selecione **Serviços de Arquivos e Armazenamento**e, em seguida, selecione **Compartilhamentos** para exibir a página Compartilhamentos.
2. No bloco **Compartilhamentos**, selecione **Tarefas** e, em seguida, selecione **Novo Compartilhamento**. O Assistente Novo Compartilhamento é exibido.
3. Na página **Selecionar Perfil**, selecione **Compartilhamento SMB – Rápido**. Se você tiver o Gerenciador de Recursos de Servidor de Arquivos instalado e estiver usando propriedades de gerenciamento de pasta, em vez disso, selecione **Compartilhamento SMB – Avançado**.
4. Na página **Compartilhar Local** , selecione o servidor e o volume nos quais deseja criar o compartilhamento.
5. Na página **Nome do Compartilhamento**, digite um nome para o compartilhamento (por exemplo, **Users$** ) na caixa **Nome do compartilhamento**.
    >[!TIP]
    >Ao criar o compartilhamento, oculte-o colocando um ```$``` após o nome do compartilhamento. Isso ocultará o compartilhamento de navegadores casuais.
6. Na página **Outras Configurações**, desmarque a caixa de seleção Habilitar disponibilidade contínua (se presente) e, como opção, marque as caixas de seleção **Habilitar enumeração baseada em acesso** e **Criptografar o acesso a dados**.
7. Na página **Permissões**, clique em **Personalizar Permissões…** . A caixa de diálogo Configurações de Segurança Avançada é exibida.
8. Clique em **Desabilitar herança** e, em seguida, selecione **Converter permissões herdadas em permissões explícitas nesse objeto**.
9. Definir as permissões conforme descrito na Tabela 1 e mostrado na Figura 1, removendo as permissões para contas e grupos não listados e incluindo permissões especiais ao grupo Usuários de Redirecionamento de Pastas que você criou na Etapa 1.

    ![Como definir as permissões para o compartilhamento de pastas redirecionadas](media/deploy-folder-redirection/setting-the-permissions-for-the-redirected-folders-share.png)

    **Figura 1** Como definir as permissões para o compartilhamento de pastas redirecionadas
10. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Propriedades de Gerenciamento** , selecione o valor de Uso da Pasta de **Arquivos do Usuário** .
11. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Cota** , opcionalmente selecione uma cota para aplicar aos usuários do compartilhamento.
12. Na página **Confirmação**, selecione **Criar**.

### <a name="required-permissions-for-the-file-share-hosting-redirected-folders"></a>Permissões necessárias para o compartilhamento de arquivo que hospeda pastas redirecionadas

| Conta de Usuário  | Acesso  | Aplica-se a  |
| --------- | --------- | --------- |
| Sistema     | Controle total        |    Essa pasta, subpastas e arquivos     |
| Administradores     | Controle total       | Apenas essa pasta        |
| Criador/Proprietário     |   Controle total      |   Apenas subpastas e arquivos      |
| Grupo de segurança de usuários que precisam colocar dados no compartilhamento (Usuários de Redirecionamento de Pastas)     |   Listar pasta/ler dados *(permissões avançadas)* <p>Criar pastas/acrescentar dados *(permissões avançadas)* <p>Ler atributos *(permissões avançadas)* <p>Ler atributos estendidos *(permissões avançadas)* <p>Ler permissões *(permissões avançadas)*      |  Apenas essa pasta       |
| Outros grupos e contas     |  Nenhum (remover)       |         |

## <a name="step-3-create-a-gpo-for-folder-redirection"></a>Etapa 3: Criar GPO para Redirecionamento de Pastas

Se você ainda não tiver criado um GPO para as configurações de Redirecionamento de Pastas, use o procedimento a seguir para criar um.

Veja como criar um GPO para o Redirecionamento de Pastas:

1. Abra o Gerenciador do Servidor em um computador com o Gerenciamento de Política de Grupo instalado.
2. No menu **Ferramentas**, selecione **Gerenciamento de Política de Grupo**.
3. Clique com o botão direito do mouse no domínio ou UO em que deseja instalar o Redirecionamento de Pastas, então selecione **Criar um GPO nesse domínio e vincule-o aqui**.
4. Na caixa de diálogo **Novo GPO**, insira um nome para o GPO (por exemplo, **Configurações de Redirecionamento de Pastas**) e, em seguida, selecione **OK**.
5. Clique com o botão direito do mouse no GPO recentemente criado e, em seguida, desmarque a caixa de seleção **Vínculo habilitado** . Isso evita que o GPO seja aplicado até que você finalize a configuração dele.
6. Selecione o GPO. Na seção **Filtragem de Segurança** da guia **Escopo**, selecione **Usuários Autenticados** e, em seguida, selecione **Remover** para impedir que o GPO seja aplicado a todos.
7. Na seção **Filtro de Segurança**, selecione **Adicionar**.
8. Na caixa de diálogo **Selecionar Usuário, Computador ou Grupo**, digite o nome do grupo de segurança criado por você na Etapa 1 (por exemplo, **Usuários de Redirecionamento de Pastas**) e, em seguida, selecione **OK**.
9. Selecione a guia **Delegação**, selecione **Adicionar**, insira **Usuários Autenticados**, selecione **OK** e, em seguida, selecione **OK** novamente para aceitar as permissões de leitura padrão.

    Esta etapa é necessária devido a alterações de segurança feitas em [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016).

> [!IMPORTANT]
> Devido às alterações de segurança feitas em [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14-2016), agora você deve conceder ao grupo usuários autenticados permissões de leitura delegadas para o GPO de Redirecionamento de Pastas. Caso contrário, o GPO não será aplicado aos usuários ou, se já estiver aplicado, será removido, redirecionando as pastas de volta para o computador local. Para obter mais informações, confira [Implantar a Atualização de Segurança da Política de Grupo MS16-072](https://techcommunity.microsoft.com/t5/ask-the-directory-services-team/deploying-group-policy-security-update-ms16-072-kb3163622/ba-p/400434).

## <a name="step-4-configure-folder-redirection-with-offline-files"></a>Etapa 4: Configurar o Redirecionamento de Pastas com Arquivos Offline

Depois de criar um GPO para as configurações de Redirecionamento de Pastas, edite as configurações de Política de Grupo para habilitar e configurar o Redirecionamento de Pastas, conforme discutido no procedimento a seguir.

> [!NOTE]
> A opção Arquivos Offline está habilitada por padrão para pastas redirecionadas em computadores cliente do Windows e desabilitada em computadores que executam o Windows Server, a menos que seja alterado pelo usuário. Para usar Política de Grupo para controlar se a opção Arquivos Offline está habilitada, use a configuração de política **Permitir ou proibir o uso do recurso Arquivos Offline**.
> Para obter informações sobre algumas das outras configurações da Política de Grupo de Arquivos Offline, confira [Habilitar a Funcionalidade de Arquivos Offline Avançada](</previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn270369(v%3dws.11)>) e [Configurar a Política de Grupo para Arquivos Offline](</previous-versions/windows/it-pro/windows-server-2003/cc759721(v%3dws.10)>).

Veja como configurar o Redirecionamento de Pastas na Política de Grupo:

1. Em Gerenciamento de Política de Grupo, clique com o botão direito do mouse no GPO que você criou (por exemplo, **Configurações de Redirecionamento de Pastas**) e, em seguida, selecione **Editar**.
2. Na janela Editor de Gerenciamento de Política de Grupo, navegue até **Configuração de Usuário**, então **Políticas**, **Configurações do Windows** e **Redirecionamento de Pastas**.
3. Clique com o botão direito do mouse em uma pasta que você deseja redirecionar (por exemplo, **Documentos**) e, em seguida, selecione **Propriedades**.
4. Na caixa de diálogo **Propriedades**, na caixa **Configuração**, selecione **Básica – redirecionar a pasta de todos para a mesma localização**.

    > [!NOTE]
    > Para aplicar o Redirecionamento de Pastas a computadores cliente que executam o Windows XP ou o Windows Server 2003, selecione a guia **Configurações** e marque a caixa de seleção **Também aplicar a política de redirecionamento aos sistemas operacionais Windows 2000, Windows 2000 Server, Windows XP e Windows Server 2003**.

5. Na seção **Localização da pasta de destino**, selecione **Criar uma pasta para cada usuário no caminho raiz** e, na caixa **Caminho Raiz**, digite o caminho para o compartilhamento de arquivo que armazena pastas redirecionadas, por exemplo: **\\\\fs1.corp.contoso.com\\users$**
6. Selecione a guia **Configurações** e, na seção **Remoção de política**, opcionalmente selecione **Redirecionar a pasta de volta para a localização de userprofile local quando a política for removida** (essa configuração pode ajudar a fazer o redirecionamento de pastas se comportar de maneira mais previsível para administradores e usuários).
7. Selecione **OK** e, em seguida, selecione **Sim** na caixa de diálogo de Aviso.

## <a name="step-5-enable-the-folder-redirection-gpo"></a>Etapa 5: Habilitar o GPO de Redirecionamento de Pastas

Depois de concluir a definição das configurações de Política de Grupo de Redirecionamento de Pastas, a próxima etapa é habilitar o GPO, permitindo que ele seja aplicado aos usuários afetados.

> [!TIP]
> Se você planejar implantar suporte de computador primário ou outras configurações de política, faça isso agora, antes de habilitar o GPO. Isso evita que os dados do usuário sejam copiados para computadores não primários antes de o suporte de computador primário ser habilitado.

Veja como habilitar o GPO de Redirecionamento de Pastas:

1. Abra Gerenciamento de Política de Grupo.
2. Clique com o botão direito do mouse no GPO criado por você e, em seguida, selecione **Vínculo Habilitado**. Uma caixa de seleção será exibida ao lado do item de menu.

## <a name="step-6-test-folder-redirection"></a>Etapa 6: Testar o Redirecionamento de Pastas

Para testar o Redirecionamento de Pastas, entre em um computador com uma conta de usuário configurada para o Redirecionamento de Pastas. Em seguida, confirme se as pastas e os perfis são redirecionados.

Veja aqui como testar o Redirecionamento de Pastas:

1. Entre em um computador primário (caso seu computador primário esteja habilitado para suporte) com uma conta de usuário para a qual você tenha habilitado o Redirecionamento de Pastas.
2. Caso o usuário tenha sido anteriormente designado para o computador, abra um prompt de comandos com privilégios elevados e, em seguida, digite o comando a seguir para garantir que as configurações de Política de Grupo mais recentes sejam aplicadas ao computador cliente:

    ```PowerShell
    gpupdate /force
    ```
3. Abra o Explorador de Arquivos.
4. Clique com o botão direito do mouse em uma pasta redirecionada (por exemplo, a pasta Meus Documentos na biblioteca de Documentos) e, em seguida, selecione **Propriedades**.
5. Selecione a guia **Localização** e confirme se o caminho exibe o compartilhamento de arquivo especificado, em vez de um caminho local.

## <a name="appendix-a-checklist-for-deploying-folder-redirection"></a>Apêndice A: Lista de verificação para implantar o Redirecionamento de Pastas

| Status           | Ação |
| ---              | ---    |
| ☐<br>☐<br>☐    | 1. Preparar domínio<br>– Ingressar computadores no domínio<br>– Criar contas de usuário |
| ☐<br><br><br>   | 2. Criar grupo de segurança para Redirecionamento de Pastas<br>– Nome do grupo:<br>– Membros: |
| ☐<br><br>       | 3. Criar um compartilhamento de arquivo para pastas redirecionadas<br>– Nome do compartilhamento de arquivo: |
| ☐<br><br>       | 4. Criar GPO para Redirecionamento de Pastas<br>– Nome do GPO: |
| ☐<br><br>☐<br>☐<br>☐<br>☐<br>☐ | 5. Definir as configurações de política de Arquivos Offline e Redirecionamento de Pastas<br>– Pastas redirecionadas:<br>– Suporte para Windows 2000, Windows XP e Windows Server 2003 habilitado?<br>– Arquivos Offline habilitados? (habilitado por padrão em computadores cliente do Windows)<br>– Modo Sempre Offline habilitado?<br>– A sincronização de arquivo em segundo plano está habilitada?<br>– A movimentação otimizada de pastas redirecionadas está habilitada? |
| ☐<br><br>☐<br><br>☐<br>☐ | 6. (Opcional) Habilitar suporte ao computador primário<br>– Baseado no computador ou no usuário?<br>– Designar computadores primários para usuários<br>– Localização dos mapeamentos de computador primário e usuário:<br>– (Opcional) Habilitar suporte de computador primário para redirecionamento de pastas<br>– (Opcional) Habilitar suporte de computador primário para Perfis de Usuários Móveis |
| ☐         | 7. Habilitar o GPO de Redirecionamento de Pastas |
| ☐         | 8. Testar o Redirecionamento de Pastas |

## <a name="change-history"></a>Histórico de alterações

A tabela a seguir resume algumas das alterações mais importantes para este tópico.

| Data | Descrição | Motivo|
| --- | --- | --- |
| 18 de janeiro de 2017 | Adicionada uma etapa à [Etapa 3: Criar um GPO para Redirecionamento de Pastas](#step-3-create-a-gpo-for-folder-redirection) para delegar permissões de Leitura a Usuários Autenticados, que agora é necessário devido a uma atualização de segurança da Política de Grupo. | Comentários de clientes |

## <a name="more-information"></a>Mais informações

* [Redirecionamento de Pastas, Arquivos Offline e Perfis de Usuários Móveis](folder-redirection-rup-overview.md)
* [Implantar computadores primários para Redirecionamento de Pastas e Perfis de Usuários Móveis](deploy-primary-computers.md)
* [Habilitar a funcionalidade de Arquivos Offline avançada](enable-always-offline.md)
* [Declaração de suporte da Microsoft sobre dados de perfil do usuário replicados](/archive/blogs/askds/microsofts-support-statement-around-replicated-user-profile-data)
* [Realizar o sideload de aplicativos com o DISM](</previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
* [Solução de problemas de empacotamento, implantação e consulta de aplicativos baseados em Windows Runtime](/windows/win32/appxpkg/troubleshooting)
