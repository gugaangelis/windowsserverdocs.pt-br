---
title: Implantando perfis de usuário de roaming
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: 1fcabf890c0c54e12c1650c31a072d17a33e292f
ms.sourcegitcommit: 23a6e83b688119c9357262b6815c9402c2965472
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69560547"
---
# <a name="deploying-roaming-user-profiles"></a>Implantando perfis de usuário de roaming

>Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Este tópico descreve como usar o Windows Server para implantar [perfis de usuário](folder-redirection-rup-overview.md) de roaming em computadores cliente Windows. Perfis de usuário de roaming redireciona perfis de usuário para um compartilhamento de arquivos para que os usuários recebam as mesmas configurações de sistema operacional e aplicativo em vários computadores.

Para obter uma lista de alterações recentes neste tópico, consulte a seção [histórico de alterações](#change-history) deste tópico.

> [!IMPORTANT]
> Devido às alterações de segurança feitas no [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), [atualizamos a etapa 4: Opcionalmente, crie um GPO para perfis](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) de usuário móvel neste tópico para que o Windows possa aplicar corretamente a política de perfis de usuário de roaming (e não reverter para políticas locais em PCs afetados).

> [!IMPORTANT]
>  As personalizações do usuário para iniciar são perdidas após uma atualização in-loco do sistema operacional na seguinte configuração:
> - Os usuários estão configurados para um perfil móvel
> - Os usuários têm permissão para fazer alterações no início
>
> Como resultado, o menu iniciar é redefinido para o padrão da nova versão do sistema operacional após a atualização in-loco do sistema operacional. Para soluções alternativas, consulte [o Apêndice C: Contornando layouts de menu iniciar redefinição após atualizações](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="hardware-requirements"></a>Requisitos de hardware

Perfis de usuário de roaming exigem um computador baseado em x64 ou x86; Não há suporte para ele no Windows RT.

### <a name="software-requirements"></a>Requisitos de software

Os Perfis de Usuário Móvel possuem os seguintes requisitos de software:

- Se estiver implantando os Perfis de Usuário Móvel com Redirecionamento de Pasta em um ambiente com perfis de usuários locais existentes, implante o Redirecionamento de Pasta antes dos Perfis de Usuário Móvel para minimizar o tamanho dos perfis móveis. Após as pastas de usuários existentes serem redirecionadas com êxito, você pode implantar os Perfis de Usuário Móvel.
- Para administrar os Perfis de Usuário Móvel, você deve ser registrado como um membro do grupo de segurança Administradores de Domínio, o grupo de segurança Administradores Corporativos ou o grupo de segurança Proprietários Criadores de Política de Grupo.
- Os computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Os computadores cliente devem ser integrados aos AD DS (Serviços de Domínio Active Directory) que você está gerenciando.
- Um computador deve ser disponibilizado com o Gerenciamento de Política de Grupo e o Centro de Administração do Active Directory instalados.
- Um servidor de arquivos deve estar disponível para hospedar perfis de usuário móvel.

    - Se o compartilhamento de arquivos usar os Namespaces do DFS, as pastas DFS (links) deverão ter um único destino para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Se o compartilhamento de arquivos usar a Replicação do DFS para replicar o conteúdo com outro servidor, os usuários devem poder acessar apenas o servidor de origem para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Se o compartilhamento de arquivos for clusterizado, desabilite a disponibilidade contínua no compartilhamento de arquivos para evitar problemas de desempenho.
- Para usar o suporte a computadores primários em perfis de usuário de roaming, há requisitos adicionais de computador cliente e de esquema de Active Directory. Para obter mais informações, consulte [implantar computadores primários para redirecionamento de pasta e perfis de usuário de roaming](deploy-primary-computers.md).
- O layout do menu iniciar de um usuário não fará roaming no Windows 10, Windows Server 2019 ou Windows Server 2016 se eles estiverem usando mais de um PC, Host da Sessão da Área de Trabalho Remota ou um servidor de VDI (infraestrutura de área de trabalho virtualizada). Como alternativa, você pode especificar um layout de início, conforme descrito neste tópico. Ou você pode fazer uso de discos de perfil do usuário, o que transfere adequadamente as configurações do menu iniciar quando usadas com servidores de Host da Sessão da Área de Trabalho Remota ou servidores de VDI. Para obter mais informações, consulte [mais fácil gerenciamento de dados de usuário com discos de perfil de usuário no Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Considerações ao usar os Perfis de Usuário Móvel em diversas versões do Windows

Se você decidir usar os Perfis de Usuário Móvel em diferentes versões do Windows, recomendamos executar as seguintes ações:

- Configure o Windows para manter versões de perfil separadas para cada versão do sistema operacional. Isso ajuda a prevenir problemas indesejáveis e imprevisíveis, como perfis corrompidos.
- Use o Redirecionamento de Pastas para armazenar arquivos de usuários, como documentos e imagens fora dos perfis de usuário. Isso permite que os mesmos arquivos estejam disponíveis aos usuários nas versões do sistema operacional. Isso também mantém os perfis menores e a conexão mais rápida.
- Alocar armazenamento suficiente para os Perfis de Usuário Móvel. Se você der suporte a duas versões do sistema operacional, os perfis dobrarão em número (e, portanto, o espaço total consumido), pois um perfil separado é mantido para cada versão do sistema operacional.
- Não use perfis de usuário de roaming em computadores que executam o Windows Vista/Windows Server 2008 e Windows 7/Windows Server 2008 R2. O roaming entre essas versões de sistema operacional não tem suporte devido a incompatibilidades em suas versões de perfil.
- Informe aos usuários que as alterações feitas em uma versão do sistema operacional não irão para outra versão do sistema operacional.
- Ao mover seu ambiente para uma versão do Windows que usa uma versão de perfil diferente (como do Windows 10 para o Windows 10, versão 1607 — [consulte o apêndice B: Informações](#appendix-b-profile-version-reference-information) de referência da versão do perfil para uma lista), os usuários recebem um perfil de usuário móvel novo e vazio. Você pode minimizar o impacto de obter um novo perfil usando o redirecionamento de pasta para redirecionar pastas comuns. Não há um método com suporte para migrar perfis de usuário de roaming de uma versão de perfil para outra.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Etapa 1: Habilitar o uso de versões separadas de perfil

Se você estiver implantando perfis de usuário móvel em computadores que executam o Windows 8.1, o Windows 8, o Windows Server 2012 R2 ou o Windows Server 2012, recomendamos fazer algumas alterações no ambiente do Windows antes da implantação. Essas alterações ajudam a garantir que as atualizações futuras do sistema operacional sejam suaves, o que facilita a capacidade de executar simultaneamente diversas versões do Windows com Perfis de Usuário Móvel.

Para fazer essas alterações, use o procedimento a seguir.

1. Baixe e instale a atualização de software apropriada em todos os computadores nos quais usará perfis móvel, obrigatório, superobrigatório ou padrão de domínio:

    - Windows 8.1 ou Windows Server 2012 R2: Instale a atualização de software descrita no artigo [2887595](http://support.microsoft.com/kb/2887595) na base de dados de conhecimento Microsoft (quando lançado).
    - Windows 8 ou o Windows Server 2012: instale a atualização de software descrita no artigo [2887239](http://support.microsoft.com/kb/2887239) na Base de Dados de Conhecimento Microsoft.

2. Em todos os computadores que executam Windows 8.1, Windows 8, Windows Server 2012 R2 ou Windows Server 2012 em que você usará perfis de usuário móvel, use o editor do registro ou Política de Grupo para criar o valor DWORD da chave do `1`registro a seguir e defina-o como. Para obter informações sobre como criar chaves do registro usando a política de grupo, consulte [Configurar um Item do Registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

    ```
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\ProfSvc\Parameters\UseProfilePathExtensionVersion
    ```

    > [!WARNING]
    > A edição incorreta do Registro pode causar danos graves ao sistema. Antes de alterar o Registro, faça backup de todos os dados importantes do computador.
3. Reinicie os computadores.

## <a name="step-2-create-a-roaming-user-profiles-security-group"></a>Etapa 2: Criar um grupo de segurança de Perfis de Usuário Móvel

Se seu ambiente ainda não estiver configurado com os Perfis de Usuário Móvel, a primeira etapa é criar um grupo de segurança que contenha todos os usuários e/ou computadores para os quais deseja aplicar as configurações de política dos Perfis de Usuário Móvel.

- Os administradores de implantações de perfis de usuário móvel de uso geral geralmente criam um grupo de segurança para os usuários.
- Administradores de Serviços de Área de Trabalho Remota ou implantações de área de trabalho virtualizada geralmente usam um grupo de segurança para usuários e os computadores compartilhados.

Veja como criar um grupo de segurança para perfis de usuário de roaming:

1. Abra Gerenciador do Servidor em um computador com Active Directory centro de administração instalado.
2. No menu **ferramentas** , selecione **Active Directory centro de administração**. O Centro de Administração do Active Directory é exibido.
3. Clique com o botão direito do mouse no domínio ou unidade organizacional apropriado, selecione **novo**e, em seguida, selecione **grupo**.
4. Na janela **Criar Grupo**, na seção **Grupo**, especifique as seguintes configurações:

    - Em **Nome do grupo**, digite o nome do grupo de segurança, por exemplo: **Computadores e Usuários de Perfis de Usuário Móvel**.
    - Em **escopo do grupo**, selecione **segurança**e, em seguida, selecione **global**.

5. Na seção **Membros** , selecione **Adicionar**. A caixa de diálogo Selecionar Usuários, Contatos, Computadores, Contas de Serviço ou Grupos é exibida.
6. Se você quiser incluir contas de computador no grupo de segurança, selecione **tipos de objeto**, marque a caixa de seleção **computadores** e, em seguida, selecione **OK**.
7. Digite os nomes dos usuários, grupos e/ou computadores nos quais você deseja implantar perfis de usuário de roaming, selecione **OK**e, em seguida, selecione **OK** novamente.

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Etapa 3: Criar um compartilhamento de arquivos para perfis de usuário móvel

Se você ainda não tiver um compartilhamento de arquivos separado para perfis de usuário de roaming (independente de quaisquer compartilhamentos para pastas redirecionadas para evitar o cache inadvertido da pasta de perfil móvel), use o procedimento a seguir para criar um compartilhamento de arquivos em um servidor que executa o Windows Servidor.

> [!NOTE]
> Algumas funcionalidades podem diferir ou ficar indisponíveis dependendo da versão do Windows Server que você está usando.

Veja como criar um compartilhamento de arquivos no Windows Server:

1. No painel de navegação Gerenciador do Servidor, selecione **serviços de arquivo e armazenamento**e, em seguida, selecione compartilhamentos para exibir a página compartilhamentos.
2. No bloco compartilhamentos, selecione **tarefas**e, em seguida, selecione **novo compartilhamento**. O Assistente Novo Compartilhamento é exibido.
3. Na página **selecionar perfil** , selecione **compartilhamento SMB – rápido**. Se você tiver o Gerenciador de recursos de servidor de arquivos instalado e estiver usando as propriedades de gerenciamento de pastas, selecione **compartilhamento SMB – avançado**.
4. Na página **Compartilhar Local**, selecione o servidor e o volume nos quais deseja criar o compartilhamento.
5. Na página **Compartilhar Nome** , digite um nome para o compartilhamento (por exemplo, **User Profiles$** ) na caixa **Compartilhar nome** .

    > [!TIP]
    > Ao criar o compartilhamento, oculte o compartilhamento colocando um ```$``` após o nome do compartilhamento. Isso oculta o compartilhamento de navegadores casuais.

6. Na página **Outras Configurações**, desmarque a caixa de seleção **Habilitar disponibilidade contínua**, se presente, e, opcionalmente, marque as caixas de seleção **Habilitar enumeração baseada em acesso** e **Criptografar o acesso a dados**.
7. Na página **permissões** , selecione **Personalizar permissões...** . A caixa de diálogo Configurações de Segurança Avançada é exibida.
8. Selecione **desabilitar herança**e, em seguida, selecione **converter permissões herdadas em permissão explícita neste objeto**.
9. Defina as permissões conforme descrito em [permissões necessárias para o compartilhamento de arquivos que hospeda perfis de usuário móveis](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) e mostrado na captura de tela a seguir, removendo permissões para grupos e contas não listadas e adicionando permissões especiais ao usuário móvel Perfis usuários e computadores que você criou na etapa 1.
    
    ![Janela configurações de segurança avançadas mostrando permissões conforme descrito na tabela 1](media/advanced-security-user-profiles.jpg)
    
    **Figura 1** Definir as permissões para o compartilhamento de perfis de usuário móvel
10. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Propriedades de Gerenciamento** , selecione o valor de Uso da Pasta de **Arquivos do Usuário** .
11. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Cota** , opcionalmente selecione uma cota para aplicar aos usuários do compartilhamento.
12. Na página **confirmação** , selecione **criar.**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Permissões necessárias para o compartilhamento de arquivos que hospeda perfis de usuário móvel

| Conta de Usuário | Access | Aplica-se a |
|   -   |   -   |   -   |
|   Sistema    |  Controle total     |  Essa pasta, subpastas e arquivos     |
|  Administradores     |  Controle total     |  Apenas essa pasta     |
|  Criador/Proprietário     |  Controle total     |  Apenas subpastas e arquivos     |
| Grupo de segurança de usuários que precisam colocar dados no compartilhamento (Computadores e Usuários de Perfis de Usuário Móvel)      |  Listar pasta/ler dados *(permissões avançadas)* <br />Criar pastas/acrescentar dados *(permissões avançadas)* |  Apenas essa pasta     |
| Outros grupos e contas   |  Nenhum (remover)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Etapa 4: Opcionalmente, criar um GPO para Perfis de Usuário Móvel

Se você ainda não possui um GPO criado para as configurações de Perfis de Usuário Móvel, use o procedimento a seguir para criar um GPO vazio para ser usado com os Perfis de Usuário Móvel. Esse GPO permite a você definir configurações de Perfis de Usuário Móvel (como suporte ao computador primário, que é discutido separadamente) e também pode ser usado para habilitar os Perfis de Usuário Móvel em computadores, como é feito geralmente ao implantar em ambientes de área de trabalho virtualizada ou com Serviços de Área de Trabalho Remota.

Veja como criar um GPO para perfis de usuário de roaming:

1. Abra o Gerenciador do Servidor em um computador com o Gerenciamento de Política de Grupo instalado.
2. No menu **ferramentas** , selecione **Gerenciamento de política de grupo**. O Gerenciamento de Política de Grupo é exibido.
3. Clique com o botão direito do mouse no domínio ou na unidade organizacional em que você deseja configurar perfis de usuário móvel, selecione **criar um GPO nesse domínio e vincule-o aqui**.
4. Na caixa de diálogo **novo GPO** , digite um nome para o GPO (por exemplo, **configurações de perfil de usuário móvel**) e selecione **OK**.
5. Clique com o botão direito do mouse no GPO recentemente criado e, em seguida, desmarque a caixa de seleção **Vínculo habilitado** . Isso evita que o GPO seja aplicado até que você finalize a configuração dele.
6. Selecione o GPO. Na seção **filtragem de segurança** da guia **escopo** , selecione **usuários autenticados**e, em seguida, selecione **remover** para impedir que o GPO seja aplicado a todos.
7. Na seção **filtragem de segurança** , selecione **Adicionar**.
8. Na caixa de diálogo **Selecionar usuário, computador ou grupo** , digite o nome do grupo de segurança criado na etapa 1 (por exemplo, perfis de **usuário móvel usuários e computadores**) e, em seguida, selecione **OK**.
9. Selecione a guia **delegação** , selecione **Adicionar**, digite **usuários autenticados**, selecione **OK**e, em seguida, selecione **OK** novamente para aceitar as permissões de leitura padrão.
    
    Esta etapa é necessária devido a alterações de segurança feitas em [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>Devido às alterações de segurança feitas no [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), agora você deve conceder permissões de leitura delegadas ao grupo de usuários autenticados para o GPO-caso contrário, o GPO não será aplicado aos usuários ou, se já estiver aplicado, o GPO será removido, redirecionando os perfis de usuário de volta para o PC local. Para obter mais informações, consulte Implantando [política de grupo atualização de segurança MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Etapa 5: Opcionalmente, configurar perfis de usuário móvel em contas de usuários

Se você estiver implantando os Perfis de Usuário Móvel nas contas de usuários, use o procedimento a seguir para especificar os perfis de usuário móvel para as contas de usuário nos Serviços de Domínio Active Directory. Se você estiver implantando perfis de usuário de roaming em computadores, como normalmente é feito para implantações de serviços de área de trabalho remota ou área de trabalho virtualizada [, use o procedimento documentado na etapa 6: Opcionalmente, configure perfis de usuário móveis em computadores](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

> [!NOTE]
> Se você configurar os Perfis de Usuário Móvel em contas de usuário usando o Active Directory e em computadores usando a Política de Grupo, a configuração baseada em computadores terá precedência.

Veja como configurar perfis de usuário móvel em contas de usuário:

1. No Centro de Administração do Active Directory, navegue até o contêiner **Usuários** (ou UO) no domínio apropriado.
2. Selecione todos os usuários aos quais você deseja atribuir um perfil de usuário de roaming, clique com o botão direito do mouse em usuários e selecione **Propriedades**.
3. Na seção **perfil** , marque a caixa de seleção **caminho do perfil:** e insira o caminho para o compartilhamento de arquivos em que você deseja armazenar o perfil de usuário de roaming do `%username%` usuário, seguido de (que é substituído automaticamente pelo nome de usuário que o primeiro hora em que o usuário entra). Por exemplo:
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Para especificar um perfil de usuário móvel obrigatório, especifique o caminho para o arquivo NTuser. Man que você criou anteriormente, por exemplo, `fs1.corp.contoso.comUser Profiles$default`. Para obter mais informações, consulte [criar perfis de usuário obrigatórios](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Selecione **OK**.

> [!NOTE]
> Por padrão, a implantação de todos os aplicativos com base no Tempo de Execução do Windows® (Windows Store) é permitida ao usar os Perfis de Usuário Móvel. No entanto, ao usar um perfil especial, os aplicativos não são implantados por padrão. Os perfis especiais são perfis de usuários nos quais as alterações são descartadas após o usuário se registrar:
> <br><br>Para remover as restrições na implantação do aplicativo para perfis especiais, habilite a configuração de política **Permitir operações de implantação em perfis especiais** (localizada em Computer Configuration\Policies\Administrative Templates\Windows Components\App Package Deployment). No entanto, os aplicativos implantados nesse cenário deixarão alguns dados armazenados no computador, que poderia criar acúmulos, por exemplo, se houvesse centenas de usuários em um único computador. Para limpar aplicativos, localize ou desenvolva uma ferramenta que use a API [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) para limpar os pacotes de aplicativos para usuários que não têm mais um perfil no computador.
> <br><br>Para mais informações em segundo plano sobre os aplicativos da Windows Store, consulte [Gerenciar o acesso de clientes à Windows Store](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Etapa 6: Opcionalmente, configurar perfis de usuário móvel em computadores

Se estiver implantando os Perfis de Usuário Móvel para computadores, como normalmente é feito para as implantações de Serviços de área de trabalho remota ou de área de trabalho virtualizada, use o procedimento a seguir. Se você estiver implantando perfis de usuário de roaming para contas de usuário, use o [procedimento descrito na etapa 5: Opcionalmente, configure perfis de usuário móveis em contas](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts)de usuário.

Você pode usar Política de Grupo para aplicar perfis de usuário de roaming a computadores que executam o Windows 8.1, o Windows 8, o Windows 7, o Windows Vista, o Windows Server 2019, o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2008 R2 ou o Windows Server 2008.

> [!NOTE]
> Se você configurar Perfis de Usuário Móvel em computadores usando a Política de Grupo e em contas de usuários usando o Active Directory, a configuração de políticas com base em computadores terá precedência.

Veja como configurar perfis de usuário móveis em computadores:

1. Abra o Gerenciador do Servidor em um computador com o Gerenciamento de Política de Grupo instalado.
2. No menu **ferramentas** , selecione **Gerenciamento de política de grupo**. O gerenciamento de Política de Grupo será exibido.
3. No gerenciamento de Política de Grupo, clique com o botão direito do mouse no GPO criado na etapa 3 (por exemplo, **configurações de perfis de usuário móvel**) e selecione **Editar**.
4. Na janela Editor de Gerenciamento de Política de Grupo, navegue até **Configuração do Computador**, em seguida, **Políticas**, **Modelos Administrativos**, **Sistema** e **Perfis de Usuário**.
5. Clique com o botão direito do mouse em **Definir caminho de perfil móvel para todos os usuários que fazem logon neste computador** e selecione **Editar**.
    > [!TIP]
    > Uma pasta base do usuário, se configurada, é a pasta padrão usada por alguns programas como o Windows PowerShell. É possível configurar um local alternativo ou local de rede por usuário usando a seção **Pasta base** das propriedades de conta do usuário em AD DS. Para configurar o local da pasta base para todos os usuários de um computador executando o Windows 8.1, o Windows 8, o Windows Server 2019, o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 em um ambiente de área de trabalho virtual, habilite a **pasta base do usuário definido** configuração de política e, em seguida, especifique o compartilhamento de arquivos e a letra da unidade para mapear (ou especificar uma pasta local). Não use elipses ou variáveis de ambiente. O alias do usuário é anexado ao final do caminho especificado durante o registro do usuário.
6. Na caixa de diálogo **Propriedades** , selecione **habilitado**
7. Na caixa **os usuários fazendo logon neste computador devem usar este caminho de perfil móvel** , insira o caminho para o compartilhamento de arquivos em que você deseja armazenar o perfil de usuário móvel do usuário, seguido `%username%` de (que é substituído automaticamente pelo nome de usuário que o primeira vez que o usuário entrar). Por exemplo:

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Para especificar um perfil de usuário móvel obrigatório, que é um perfil pré-configurado para o qual os usuários não podem fazer alterações permanentes (as alterações são redefinidas quando o usuário se desconecta), especifique o caminho para o arquivo NTuser. Man que você criou `\\fs1.corp.contoso.com\User Profiles$\default`anteriormente, por exemplo,. Para obter mais informações, consulte [Criar um perfil de usuário obrigatório](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Selecione **OK**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Etapa 7: Opcionalmente, especifique um layout de início para PCs com Windows 10

Você pode usar Política de Grupo para aplicar um layout de menu iniciar específico para que os usuários vejam o mesmo layout inicial em todos os PCs. Se os usuários entrarem em mais de um computador e você quiser que eles tenham um layout de início consistente entre PCs, certifique-se de que o GPO se aplica a todos os seus computadores.

Para especificar um layout de início, faça o seguinte:

1. Atualize seus PCs com Windows 10 para o Windows 10 versão 1607 (também conhecido como atualização de aniversário) ou mais recente e instale a atualização cumulativa de 14 de março de 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) ou mais recente.
2. Crie um arquivo XML completo ou parcial de layout do menu iniciar. Para fazer isso, consulte [Personalizar e exportar o layout inicial](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Se você especificar um layout de início *completo* , um usuário não poderá personalizar nenhuma parte do menu iniciar. Se você especificar um layout de início *parcial* , os usuários poderão personalizar tudo, exceto os grupos de blocos bloqueados que você especificar. No entanto, com um layout de início parcial, as personalizações do usuário no menu Iniciar não serão transferidas para outros computadores.
3. Use Política de Grupo para aplicar o layout de início personalizado ao GPO que você criou para perfis de usuário de roaming. Para fazer isso, consulte [usar política de grupo para aplicar um layout de início personalizado em um domínio](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Use Política de Grupo para definir o valor do registro a seguir em seus PCs com Windows 10. Para fazer isso, consulte [configurar um item de registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Ação**   | **Atualização**                  |
| ------------ | ------------                |
| Sessão         | **HKEY_LOCAL_MACHINE**      |
| Caminho da chave     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nome do valor   | **SpecialRoamingOverrideAllowed** |
| Tipo de valor   | **REG_DWORD**               |
| os dados de Valor   | **1** (ou **0** para desabilitar) |
| Base         | **Decimal**                 |

5. Adicional Habilite otimizações de logon primeiras para tornar a entrada mais rápida para os usuários. Para fazer isso, consulte [aplicar políticas para melhorar o tempo de entrada](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. Adicional Diminua ainda mais os tempos de entrada removendo aplicativos desnecessários da imagem base do Windows 10 que você usa para implantar computadores cliente. O Windows Server 2019 e o Windows Server 2016 não têm nenhum aplicativo previamente provisionado, portanto, você pode ignorar esta etapa em imagens do servidor.
    - Para remover aplicativos, use o cmdlet [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) no Windows PowerShell para desinstalar os aplicativos a seguir. Se seus computadores já estiverem implantados, você poderá gerar scripts para a remoção desses aplicativos usando o [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
      - Microsoft. windowscommunicationsapps\_8wekyb3d8bbwe
      - Microsoft. BingWeather\_8wekyb3d8bbwe
      - Microsoft. DesktopAppInstaller\_8wekyb3d8bbwe
      - Microsoft. getstarted\_8wekyb3d8bbwe
      - Microsoft. Windows. photos\_8wekyb3d8bbwe
      - Microsoft. WindowsCamera\_8wekyb3d8bbwe
      - Microsoft. WindowsFeedbackHub\_8wekyb3d8bbwe
      - Microsoft. WindowsStore\_8wekyb3d8bbwe
      - Microsoft. XboxApp\_8wekyb3d8bbwe
      - Microsoft. XboxIdentityProvider\_8wekyb3d8bbwe
      - Microsoft. ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>A desinstalação desses aplicativos diminui os tempos de entrada, mas você pode deixá-los instalados se sua implantação precisar de qualquer um deles.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Etapa 8: Habilitar os GPO de perfis de usuário móvel

Se você configurou os Perfis de Usuário Móvel em computadores usando a Política de Grupo ou se você personalizou outras configurações de Perfis de Usuário Móvel usando a Política de Grupo, a próxima etapa será habilitar o GPO, permitindo que ele seja aplicado aos usuários afetados.

>[!TIP]
>Se você planeja implementar o suporte de computador primário, faça isso agora, antes de habilitar o GPO. Isso evita que os dados do usuário sejam copiados para computadores não primários antes de o suporte de computador primário ser habilitado. Para obter as configurações de política específicas, consulte [implantar computadores primários para redirecionamento de pasta e perfis de usuário](deploy-primary-computers.md)de roaming.

Veja como habilitar o GPO de perfil de usuário móvel:

1. Abra Gerenciamento de Política de Grupo.
2. Clique com o botão direito do mouse no GPO que você criou e selecione **link habilitado**. Uma caixa de seleção é exibida próximo ao item de menu.

## <a name="step-9-test-roaming-user-profiles"></a>Etapa 9: Testar perfis de usuário móvel

Para testar os Perfis de Usuário Móvel, conecte-se a um computador com uma conta de usuário configurada para Perfis de Usuário Móvel ou conecte-se a um computador configurado para Perfis de Usuário Móvel. Em seguida, confirme se o perfil foi redirecionado.

Veja como testar perfis de usuário móvel:

1. Conecte-se a um computador primário (caso seu computador primário esteja habilitado para suporte) com uma conta de usuário para a qual você tenha os Perfis de Usuário Móvel habilitados. Se você tiver habilitado os Perfis de Usuário Móvel em computadores específicos, conecte-se a um desses computadores.
2. Caso o usuário tenha sido anteriormente designado para o computador, abra um prompt de comandos com privilégios elevados e, em seguida, digite o comando a seguir para garantir que as configurações de Política de Grupo mais recentes sejam aplicadas ao computador cliente:
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Para confirmar se o perfil do usuário está em roaming, abra o **painel de controle**, selecione **sistema e segurança**, selecione **sistema**, selecione **Configurações avançadas do sistema**, selecione **configurações** na seção perfis de usuário e procure  **Roaming** na coluna **Type** .

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Apêndice A: Lista de verificação para a implantação de Perfis de Usuário Móvel

| Status                     | Action                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Preparar domínio<br>-Ingressar computadores no domínio<br>-Habilitar o uso de versões de perfil separadas<br>-Criar contas de usuário<br>-(Opcional) implantar redirecionamento de pasta |
| ☐<br><br><br>             | 2. Criar grupo de segurança para Perfis de Usuário Móvel<br>-Nome do Grupo:<br>Os |
| ☐<br><br>                 | 3. Criar um compartilhamento de arquivos para Perfis de Usuário Móvel<br>-Nome do compartilhamento de arquivos: |
| ☐<br><br>                 | 4. Criar um GPO para perfis de usuário móvel<br>-Nome do GPO:|
| ☐                         | 5. Definir configurações de política dos Perfis de Usuário Móvel    |
| ☐<br>☐<br>☐              | 6. Habilitar perfis de usuário de roaming<br>-Habilitado em AD DS em contas de usuário?<br>-Habilitado em Política de Grupo em contas de computador?<br> |
| ☐                         | 7. Adicional Especificar um layout de início obrigatório para PCs com Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. Adicional Habilitar o suporte ao computador primário<br>-Designar computadores primários para usuários<br>-Local dos mapeamentos do usuário e do computador primário:<br>-(Opcional) habilitar o suporte ao computador primário para o redirecionamento de pasta<br>-Baseado em computador ou em um usuário?<br>-(Opcional) habilitar o suporte a computadores primários para perfis de usuário de roaming |
| ☐                        | 9. Habilitar os GPO de perfis de usuário móvel                |
| ☐                        | 10. Testar perfis de usuário móvel                         |

## <a name="appendix-b-profile-version-reference-information"></a>Apêndice B: Informações de referência de versão de perfil

Cada perfil tem uma versão de perfil que corresponde aproximadamente à versão do Windows na qual o perfil é usado. Por exemplo, o Windows 10, a versão 1703 e a versão 1607 usam o. Versão do perfil v6. A Microsoft cria uma nova versão de perfil somente quando necessário para manter a compatibilidade, motivo pelo qual nem todas as versões do Windows incluem uma nova versão de perfil.

A tabela a seguir lista a localização de Perfis de Usuário Móvel em diversas versões do Windows.

| Versão do sistema operacional | Localização do Perfil de Usuário Móvel |
| --- | --- |
| Windows XP e Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista e Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 e Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 e Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3```(depois que a atualização de software e a chave do registro forem aplicadas)<br>```\\<servername>\<fileshare>\<username>.V2```(antes que a atualização de software e a chave do registro sejam aplicadas) |
| Windows 8.1 e Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4```(depois que a atualização de software e a chave do registro forem aplicadas)<br>```\\<servername>\<fileshare>\<username>.V2```(antes que a atualização de software e a chave do registro sejam aplicadas) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10, versão 1703 e versão 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Apêndice C: Contornando layouts de menu iniciar redefinição após atualizações

Aqui estão algumas maneiras de contornar os layouts do menu iniciar sendo redefinidos após uma atualização in-loco:

- Se apenas um usuário usar o dispositivo e o administrador de ti usar uma estratégia de implantação de sistema operacional gerenciado, como o SCCM, ele poderá fazer o seguinte:
    
  1. Exportar o layout do menu iniciar com Export-Startlayout antes da atualização 
  2. Importe o layout do menu iniciar com Import-StartLayout após o OOBE, mas antes de o usuário entrar  
 
     > [!NOTE] 
     > A importação de um StartLayout modifica o perfil de usuário padrão. Todos os perfis de usuário criados após a importação receberão o layout de início importado.
 
- Os administradores de ti podem optar por gerenciar o layout do início com Política de Grupo. O uso de Política de Grupo fornece uma solução de gerenciamento centralizada para aplicar um layout inicial padronizado aos usuários. Há dois modos para os modos de usar Política de Grupo para gerenciamento inicial. Bloqueio completo e bloqueio parcial. O cenário de bloqueio completo impede que o usuário faça alterações no layout do início. O cenário de bloqueio parcial permite que o usuário faça alterações em uma área específica de iniciar. Para obter mais informações, consulte [Personalizar e exportar o layout inicial](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > O usuário fez alterações no cenário de bloqueio parcial ainda será perdido durante a atualização.

- Permitir que a redefinição do layout inicial ocorra e permitir que os usuários finais reconfigurem o início. Um email de notificação ou outra notificação pode ser enviado aos usuários finais para esperar que seus layouts de início sejam redefinidos após a atualização do sistema operacional para minimizar o impacto. 

# <a name="change-history"></a>Histórico de alterações

A tabela a seguir resume algumas das alterações mais importantes para este tópico.

| Date | Descrição |Reason|
| --- | ---         | ---   |
| 1º de maio, 2019 | Atualizações adicionadas para o Windows Server 2019 |
| 10 de abril de 2018 | Adicionada uma discussão sobre quando as personalizações do usuário a serem iniciadas são perdidas após uma atualização in-loco do sistema operacional|Problema de texto explicativo conhecido. |
| 13 de março, 2018 | Atualizado para o Windows Server 2016 | Movido para fora da biblioteca de versões anteriores e atualizado para a versão atual do Windows Server. |
| 13 de abril de 2017 | Informações de perfil adicionadas para o Windows 10, versão 1703 e esclarecidas sobre como as versões de perfil móvel funcionam ao atualizar sistemas operacionais — consulte [considerações ao usar perfis de usuário de roaming em várias versões do Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Comentários do cliente. |
| 14 de março de 2017 | Adicionada etapa opcional para especificar um layout de início obrigatório para PCs com Windows [10 no apêndice a: Lista de verificação para implantar perfis](#appendix-a-checklist-for-deploying-roaming-user-profiles)de usuário móvel. |Alterações de recurso no Windows Update mais recente. |
| 23 de janeiro de 2017 | Adicionada uma etapa à [etapa 4: Opcionalmente, crie um GPO para perfis](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) de usuário de roaming para delegar permissões de leitura a usuários autenticados, que agora é necessário devido a uma atualização de segurança política de grupo.|Alterações de segurança no processamento de Política de Grupo. |
| 29 de dezembro de 2016 | Adicionado um link na [etapa 8: Habilite o GPO](#step-8-enable-the-roaming-user-profiles-gpo) perfis de usuário de roaming para facilitar a obtenção de informações sobre como definir política de grupo para computadores primários. Também corrigimos algumas referências às etapas 5 e 6 que tinham os números errados.|Comentários do cliente. |
| 5 de dezembro de 2016 | Informações adicionadas explicando um problema de roaming das configurações do menu iniciar. | Comentários do cliente. |
| 6 de julho de 2016 | Foram adicionados sufixos de versão de [perfil do Windows 10 no apêndice B: Informações](#appendix-b-profile-version-reference-information)de referência da versão do perfil. O Windows XP e o Windows Server 2003 também foram removidos da lista de sistemas operacionais com suporte. | Atualizações para as novas versões do Windows e removeu informações sobre versões do Windows que não têm mais suporte. |
| 7 de julho de 2015 | Adicionado o requisito e a etapa para desabilitar a disponibilidade contínua ao usar um servidor de arquivos em cluster. | Compartilhamentos de arquivos em cluster têm desempenho melhor para pequenas gravações (que são típicas com perfis de usuário móvel) quando a disponibilidade contínua está desabilitada. |
| 19 de março de 2014 | Sufixos de versão de perfil em letras maiúsculas (. V2,. V3,. V4) no [apêndice B: Informações](#appendix-b-profile-version-reference-information)de referência da versão do perfil. | Embora o Windows não faça distinção entre maiúsculas e minúsculas, se você usar o NFS com o compartilhamento de arquivos, é importante ter a capitalização correta (maiúscula) para o sufixo do perfil. |
| 9 de outubro de 2013 | Revisado para o Windows Server 2012 R2 e Windows 8.1, esclarecidos algumas coisas e adicionou as [considerações ao usar perfis de usuário móvel em várias versões do Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) e [do apêndice B: Seções de informações](#appendix-b-profile-version-reference-information) de referência da versão do perfil. | Atualizações para a nova versão; Comentários do cliente. |

## <a name="more-information"></a>Mais informações

- [Implantar perfis de usuário de redirecionamento de pasta, Arquivos Offline e roaming](deploy-folder-redirection.md)
- [Implantar computadores primários para redirecionamento de pasta e perfis de usuário de roaming](deploy-primary-computers.md)
- [Implementando o gerenciamento de estado do usuário](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Declaração de suporte da Microsoft sobre dados de perfil de usuário replicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Aplicativos Sideload com o DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Solução de problemas de empacotamento, implantação e consulta de aplicativos baseados em Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)