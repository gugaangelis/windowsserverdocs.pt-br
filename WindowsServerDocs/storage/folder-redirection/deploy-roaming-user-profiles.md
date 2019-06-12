---
Title: Implantando perfis de usuário móvel
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: e6e2e32ff9aeb1b3bcfc8fed9027c7e92e13b118
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812490"
---
# <a name="deploying-roaming-user-profiles"></a>Implantando perfis de usuário móvel

>Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (canal semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Este tópico descreve como usar o Windows Server para implantar [perfis de usuário móvel](folder-redirection-rup-overview.md) aos computadores de cliente do Windows. Perfis de usuário móvel redireciona os perfis de usuário para um compartilhamento de arquivos para que os usuários recebem o mesmo sistema operacional e configurações do aplicativo em vários computadores.

Para obter uma lista das alterações recentes a este tópico, consulte o [histórico de alterações](#change-history) seção deste tópico.

> [!IMPORTANT]
> Devido às alterações de segurança feitas na [MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), atualizamos [etapa 4: Opcionalmente, criar um GPO para perfis de usuário móvel](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) neste tópico para que o Windows possa aplicar corretamente a política de perfis de usuário móvel (e não reverter para diretivas locais nos computadores afetados).

> [!IMPORTANT]
>  As personalizações do usuário inicial é perdida após uma atualização in-loco do sistema operacional na configuração a seguir:
> - Os usuários estão configurados para um perfil móvel
> - Os usuários têm permissão para fazer alterações ao início
>
> Como resultado, o menu Iniciar é redefinido para o padrão da nova versão do sistema operacional depois que o sistema operacional de atualização no local. Para soluções alternativas, consulte [apêndice c: Trabalhar em torno de redefinição de layouts de menu Iniciar após a atualização de](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="hardware-requirements"></a>Requisitos de hardware

Perfis de usuário móvel requer um computador com base em x86 ou x64; Isso não é suportado pelo RT Windows.

### <a name="software-requirements"></a>Requisitos de software

Os Perfis de Usuário Móvel possuem os seguintes requisitos de software:

- Se estiver implantando os Perfis de Usuário Móvel com Redirecionamento de Pasta em um ambiente com perfis de usuários locais existentes, implante o Redirecionamento de Pasta antes dos Perfis de Usuário Móvel para minimizar o tamanho dos perfis móveis. Após as pastas de usuários existentes serem redirecionadas com êxito, você pode implantar os Perfis de Usuário Móvel.
- Para administrar os Perfis de Usuário Móvel, você deve ser registrado como um membro do grupo de segurança Administradores de Domínio, o grupo de segurança Administradores Corporativos ou o grupo de segurança Proprietários Criadores de Política de Grupo.
- Computadores cliente devem executar o Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.
- Os computadores cliente devem ser integrados aos AD DS (Serviços de Domínio Active Directory) que você está gerenciando.
- Um computador deve ser disponibilizado com o Gerenciamento de Política de Grupo e o Centro de Administração do Active Directory instalados.
- Um servidor de arquivos deve estar disponível para hospedar perfis de usuário móvel.

    - Se o compartilhamento de arquivos usar os Namespaces do DFS, as pastas DFS (links) deverão ter um único destino para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Se o compartilhamento de arquivos usar a Replicação do DFS para replicar o conteúdo com outro servidor, os usuários devem poder acessar apenas o servidor de origem para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Se o compartilhamento de arquivos for clusterizado, desabilite a disponibilidade contínua no compartilhamento de arquivos para evitar problemas de desempenho.
- Para usar o suporte de computador primário em perfis de usuário móvel, há o computador cliente adicionais e requisitos do esquema do Active Directory. Para obter mais informações, consulte [implantar computadores primários para redirecionamento de pasta e perfis de usuário móvel](deploy-primary-computers.md).
- O layout do menu não passarão no Windows 10, Windows Server 2019 ou Windows Server 2016, se estiver usando o servidor mais de um PC, Host da sessão da área de trabalho remota ou virtualizado Desktop Infrastructure (VDI) de início de um usuário. Como alternativa, você pode especificar um layout de iniciar, conforme descrito neste tópico. Ou você pode fazer uso de discos de perfil do usuário, o qual movimentem corretamente as configurações de menu de início quando usado com servidores de Host de sessão de área de trabalho remota ou servidores VDI. Para obter mais informações, consulte [gerenciamento de dados de usuário mais fácil com discos de perfil de usuário no Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Considerações ao usar os Perfis de Usuário Móvel em diversas versões do Windows

Se você decidir usar os Perfis de Usuário Móvel em diferentes versões do Windows, recomendamos executar as seguintes ações:

- Configure o Windows para manter versões de perfil separadas para cada versão do sistema operacional. Isso ajuda a prevenir problemas indesejáveis e imprevisíveis, como perfis corrompidos.
- Use o Redirecionamento de Pastas para armazenar arquivos de usuários, como documentos e imagens fora dos perfis de usuário. Isso permite que os mesmos arquivos estejam disponíveis aos usuários nas versões do sistema operacional. Isso também mantém os perfis menores e a conexão mais rápida.
- Alocar armazenamento suficiente para os Perfis de Usuário Móvel. Se você der suporte a duas versões do sistema operacional, os perfis dobrarão em número (e, portanto, o espaço total consumido), pois um perfil separado é mantido para cada versão do sistema operacional.
- Não use perfis de usuário móvel em computadores que executam o Windows Vista/Windows Server 2008 e Windows 7/Windows Server 2008 R2. Roaming entre essas versões de sistema operacional não tem suporte devido a incompatibilidades nas versões de perfil.
- Informe os usuários que as alterações feitas em uma versão de sistema operacional não passarão para outra versão do sistema operacional.
- Ao mover seu ambiente para uma versão do Windows que usa uma versão de perfil diferente (por exemplo, Windows 10 para Windows 10, versão 1607 — consulte [apêndice b: Informações de referência de versão de perfil](#appendix-b-profile-version-reference-information) para obter uma lista), os usuários recebem um perfil de usuário móvel nova e vazia. Você pode minimizar o impacto de obtenção de um novo perfil usando o redirecionamento de pasta para redirecionar pastas comuns. Não há um método com suporte para migrar perfis de usuário móvel de versão de um perfil para outro.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Etapa 1: Habilitar o uso de versões separadas de perfil

Se você estiver implantando perfis de usuário móvel em computadores que executam o Windows 8.1, Windows 8, Windows Server 2012 R2 ou Windows Server 2012, recomendamos fazer algumas alterações no ambiente Windows antes da implantação. Essas alterações ajudam a garantir que as atualizações futuras do sistema operacional sejam suaves, o que facilita a capacidade de executar simultaneamente diversas versões do Windows com Perfis de Usuário Móvel.

Para fazer essas alterações, use o procedimento a seguir.

1. Baixe e instale a atualização de software apropriada em todos os computadores nos quais usará perfis móvel, obrigatório, superobrigatório ou padrão de domínio:

    - Windows 8.1 ou Windows Server 2012 R2: instalar a atualização de software descrita no artigo [2887595](http://support.microsoft.com/kb/2887595) na Microsoft Knowledge Base de (quando liberado).
    - Windows 8 ou o Windows Server 2012: instale a atualização de software descrita no artigo [2887239](http://support.microsoft.com/kb/2887239) na Base de Dados de Conhecimento Microsoft.

2. Em todos os computadores que executam o Windows 8.1, Windows 8, Windows Server 2012 R2 ou Windows Server 2012 em que você usará os perfis de usuário móvel, use o Editor do registro ou valor DWORD de chave de política de grupo para criar o registro a seguir e defina-o `1`. Para obter informações sobre como criar chaves do registro usando a política de grupo, consulte [Configurar um Item do Registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

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

Aqui está como criar um grupo de segurança para perfis de usuário móvel:

1. Abra o Server Manager em um computador com o Centro de administração do Active Directory instalado.
2. Sobre o **ferramentas** menu, selecione **Centro de administração do Active Directory**. O Centro de Administração do Active Directory é exibido.
3. Clique com botão direito no domínio apropriado ou UO, selecione **New**e, em seguida, selecione **grupo**.
4. Na janela **Criar Grupo**, na seção **Grupo**, especifique as seguintes configurações:

    - Em **Nome do grupo**, digite o nome do grupo de segurança, por exemplo: **Computadores e Usuários de Perfis de Usuário Móvel**.
    - Na **escopo do grupo**, selecione **Security**e, em seguida, selecione **Global**.

5. No **membros** seção, selecione **Add**. A caixa de diálogo Selecionar Usuários, Contatos, Computadores, Contas de Serviço ou Grupos é exibida.
6. Se desejar incluir contas de computador no grupo de segurança, selecione **tipos de objetos**, selecione o **computadores** caixa de seleção e, em seguida, selecione **Okey**.
7. Digite os nomes de usuários, grupos e/ou computadores aos quais você deseja implantar perfis de usuário móvel, selecione **Okey**e, em seguida, selecione **Okey** novamente.

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Etapa 3: Criar um compartilhamento de arquivos para perfis de usuário móvel

Se você ainda não tiver um compartilhamento de arquivos separado para roaming de perfis de usuário (independentes de quaisquer outros compartilhamentos para pastas redirecionadas para evitar o armazenamento em cache inadvertido da pasta de perfil móvel), use o procedimento a seguir para criar um compartilhamento de arquivos em um servidor executando o Windows Servidor.

> [!NOTE]
> Algumas funcionalidades podem diferir ou não estar disponível dependendo da versão do Windows Server que você está usando.

Aqui está como criar um compartilhamento de arquivos no Windows Server:

1. No painel de navegação do Gerenciador do servidor, selecione **serviços de arquivo e armazenamento**e, em seguida, selecione **compartilhamentos** para exibir a página compartilhamentos.
2. No bloco compartilhamentos, selecione **tarefas**e, em seguida, selecione **novo compartilhamento**. O Assistente Novo Compartilhamento é exibido.
3. Sobre o **Selecionar perfil** , selecione **compartilhamento SMB – rápido**. Se você tiver o Gerenciador de recursos de servidor de arquivos instalado e estiver usando propriedades de gerenciamento de pasta, em vez disso, selecione **compartilhamento SMB - avançado**.
4. Na página **Compartilhar Local**, selecione o servidor e o volume nos quais deseja criar o compartilhamento.
5. Na página **Compartilhar Nome** , digite um nome para o compartilhamento (por exemplo, **User Profiles$** ) na caixa **Compartilhar nome** .

    > [!TIP]
    > Ao criar o compartilhamento, oculte o compartilhamento colocando um ```$``` após o nome do compartilhamento. Isso oculta o compartilhamento de navegadores casuais.

6. Na página **Outras Configurações**, desmarque a caixa de seleção **Habilitar disponibilidade contínua**, se presente, e, opcionalmente, marque as caixas de seleção **Habilitar enumeração baseada em acesso** e **Criptografar o acesso a dados**.
7. Sobre o **permissões** página, selecione **personalizar permissões...** . A caixa de diálogo Configurações de Segurança Avançada é exibida.
8. Selecione **desabilitar herança**e, em seguida, selecione **converter permissões de herança em permissões explícitas nesse objeto**.
9. Defina as permissões conforme descrito em [permissões necessárias para o arquivo compartilham hospeda perfis de usuário móvel](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) e mostrado na seguinte captura de tela, removendo as permissões para contas e grupos não listados e adicionando especiais permissões para o grupo de computadores e usuários de perfis de usuário móvel que você criou na etapa 1.
    
    ![Janela de configurações de segurança mostrando permissões conforme descrito na tabela 1 avançado](media/advanced-security-user-profiles.jpg)
    
    **Figura 1** Definir as permissões para o compartilhamento de perfis de usuário móvel
10. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Propriedades de Gerenciamento** , selecione o valor de Uso da Pasta de **Arquivos do Usuário** .
11. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Cota** , opcionalmente selecione uma cota para aplicar aos usuários do compartilhamento.
12. Sobre o **confirmação** página, selecione **criar.**

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>As permissões necessárias para os arquivo compartilhamento hospedagem perfis de usuário móvel

| Conta de Usuário | Acesso | Aplica-se a |
|   -   |   -   |   -   |
|   Sistema    |  Controle total     |  Essa pasta, subpastas e arquivos     |
|  Administradores     |  Controle total     |  Apenas essa pasta     |
|  Criador/Proprietário     |  Controle total     |  Apenas subpastas e arquivos     |
| Grupo de segurança de usuários que precisam colocar dados no compartilhamento (Computadores e Usuários de Perfis de Usuário Móvel)      |  Listar pastas / ler dados *(permissões avançadas)* <br />Criar pastas / anexar dados *(permissões avançadas)* |  Apenas essa pasta     |
| Outros grupos e contas   |  Nenhum (remover)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Etapa 4: Opcionalmente, criar um GPO para Perfis de Usuário Móvel

Se você ainda não possui um GPO criado para as configurações de Perfis de Usuário Móvel, use o procedimento a seguir para criar um GPO vazio para ser usado com os Perfis de Usuário Móvel. Esse GPO permite a você definir configurações de Perfis de Usuário Móvel (como suporte ao computador primário, que é discutido separadamente) e também pode ser usado para habilitar os Perfis de Usuário Móvel em computadores, como é feito geralmente ao implantar em ambientes de área de trabalho virtualizada ou com Serviços de Área de Trabalho Remota.

Aqui está como criar um GPO para perfis de usuário móvel:

1. Abra o Gerenciador do Servidor em um computador com o Gerenciamento de Política de Grupo instalado.
2. Dos **ferramentas** menu, selecione **Group Policy Management**. O Gerenciamento de Política de Grupo é exibido.
3. Clique com botão direito no domínio ou UO em que você deseja instalar os perfis de usuário móvel e, em seguida, selecione **criar um GPO neste domínio e vinculá-lo aqui**.
4. No **novo GPO** caixa de diálogo, digite um nome para o GPO (por exemplo, **configurações de perfil de usuário móvel**) e, em seguida, selecione **Okey**.
5. Clique com o botão direito do mouse no GPO recentemente criado e, em seguida, desmarque a caixa de seleção **Vínculo habilitado** . Isso evita que o GPO seja aplicado até que você finalize a configuração dele.
6. Selecione o GPO. No **filtragem de segurança** seção o **escopo** guia, selecione **Authenticated Users**e, em seguida, selecione **remover** para impedir que o GPO seja aplicada a todos.
7. No **filtragem de segurança** seção, selecione **Add**.
8. No **Selecionar usuário, computador ou grupo** caixa de diálogo, digite o nome da segurança do grupo que você criou na etapa 1 (por exemplo, **computadores e usuários de perfis de usuário móvel**) e, em seguida, selecione **Okey** .
9. Selecione o **delegação** guia, selecione **Add**, digite **Authenticated Users**, selecione **Okey**e, em seguida, selecione **Okey** novamente aceitar o padrão permissões de leitura.
    
    Essa etapa é necessária devido a alterações de segurança feitas na [MS16 072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>Devido às alterações de segurança feitas na [MS16 072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), agora você deve fornecer permissões de leitura de grupo delegado a usuários autenticados para o GPO – caso contrário, o GPO não será aplicado a usuários ou se ele já for aplicado, o GPO é removido, redirecionando os perfis de usuário para o computador local. Para obter mais informações, consulte [implantação de grupo de política de segurança atualização MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Etapa 5: Opcionalmente, configurar perfis de usuário móvel em contas de usuários

Se você estiver implantando os Perfis de Usuário Móvel nas contas de usuários, use o procedimento a seguir para especificar os perfis de usuário móvel para as contas de usuário nos Serviços de Domínio Active Directory. Se você estiver implantando perfis de usuário móvel em computadores, como normalmente é feito para serviços de área de trabalho remota ou implantações de área de trabalho virtualizadas, em vez disso, use o procedimento documentado na [etapa 6: Opcionalmente, configurar perfis de usuário móvel em computadores](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

> [!NOTE]
> Se você configurar os Perfis de Usuário Móvel em contas de usuário usando o Active Directory e em computadores usando a Política de Grupo, a configuração baseada em computadores terá precedência.

Aqui está como configurar perfis de usuário móvel em contas de usuário:

1. No Centro de Administração do Active Directory, navegue até o contêiner **Usuários** (ou UO) no domínio apropriado.
2. Selecione todos os usuários aos quais você deseja atribuir um perfil de usuário móvel, os usuários com o botão direito e, em seguida, selecione **propriedades**.
3. No **perfil** seção, selecione a **caminho do perfil:** caixa de seleção e, em seguida, insira o caminho para o compartilhamento de arquivos em que você deseja armazenar perfil de usuário móvel do usuário, seguido por `%username%` (que é substituídas automaticamente com o usuário nome na primeira vez que o usuário faz logon). Por exemplo:
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Para especificar um perfil de usuário móvel obrigatório, especifique o caminho para o arquivo Ntuser man criado anteriormente, por exemplo, `fs1.corp.contoso.comUser Profiles$default`. Para obter mais informações, consulte [criar perfis de usuário obrigatório](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Selecione **OK**.

> [!NOTE]
> Por padrão, a implantação de todos os aplicativos com base no Tempo de Execução do Windows® (Windows Store) é permitida ao usar os Perfis de Usuário Móvel. No entanto, ao usar um perfil especial, os aplicativos não são implantados por padrão. Os perfis especiais são perfis de usuários nos quais as alterações são descartadas após o usuário se registrar:
> <br><br>Para remover as restrições na implantação do aplicativo para perfis especiais, habilite a configuração de política **Permitir operações de implantação em perfis especiais** (localizada em Computer Configuration\Policies\Administrative Templates\Windows Components\App Package Deployment). No entanto, os aplicativos implantados nesse cenário deixarão alguns dados armazenados no computador, que poderia criar acúmulos, por exemplo, se houvesse centenas de usuários em um único computador. Para limpar os aplicativos, localize ou desenvolva uma ferramenta que usa o [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) API para limpar pacotes de aplicativos para usuários que não tenham um perfil no computador.
> <br><br>Para mais informações em segundo plano sobre os aplicativos da Windows Store, consulte [Gerenciar o acesso de clientes à Windows Store](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Etapa 6: Opcionalmente, configurar perfis de usuário móvel em computadores

Se estiver implantando os Perfis de Usuário Móvel para computadores, como normalmente é feito para as implantações de Serviços de área de trabalho remota ou de área de trabalho virtualizada, use o procedimento a seguir. Se você estiver implantando perfis de usuário móvel em contas de usuário, em vez disso, use o procedimento descrito em [etapa 5: Opcionalmente, configurar perfis de usuário móvel em contas de usuário](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts).

Você pode usar a diretiva de grupo para aplicar perfis de usuário móvel em computadores que executam o Windows 8.1, Windows 8, Windows 7, Windows Vista, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008.

> [!NOTE]
> Se você configurar Perfis de Usuário Móvel em computadores usando a Política de Grupo e em contas de usuários usando o Active Directory, a configuração de políticas com base em computadores terá precedência.

Aqui está como configurar perfis de usuário móvel em computadores:

1. Abra o Gerenciador do Servidor em um computador com o Gerenciamento de Política de Grupo instalado.
2. Dos **ferramentas** menu, selecione **Group Policy Management**. Gerenciamento de diretiva de grupo será exibida.
3. Em gerenciamento de diretiva de grupo, clique com botão direito no GPO criado na etapa 3 (por exemplo, **configurações de perfis de usuário móvel**) e, em seguida, selecione **editar**.
4. Na janela Editor de Gerenciamento de Política de Grupo, navegue até **Configuração do Computador**, em seguida, **Políticas**, **Modelos Administrativos**, **Sistema** e **Perfis de Usuário**.
5. Clique com botão direito **definir o caminho de perfil para todos os usuários registrados em log neste computador móvel** e, em seguida, selecione **editar**.
    > [!TIP]
    > Uma pasta base do usuário, se configurada, é a pasta padrão usada por alguns programas como o Windows PowerShell. É possível configurar um local alternativo ou local de rede por usuário usando a seção **Pasta base** das propriedades de conta do usuário em AD DS. Para configurar o local da pasta base para todos os usuários de um computador que executa o Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 ou Windows Server 2012 em um ambiente de área de trabalho virtual, habilite o **pasta base do conjunto de usuário**  configuração de política e, em seguida, especifique a letra de unidade e de compartilhamento de arquivo para mapear (ou especifique uma pasta local). Não use elipses ou variáveis de ambiente. O alias do usuário é anexado ao final do caminho especificado durante o registro do usuário.
6. No **propriedades** caixa de diálogo, selecione **habilitado**
7. No **os usuários registrados em log neste computador devem usar esse caminho de perfil móvel** , digite o caminho para o compartilhamento de arquivos em que você deseja armazenar perfil de usuário móvel do usuário, seguido por `%username%` (que é automaticamente substituído com o usuário nome na primeira vez o usuário faz logon). Por exemplo:

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Para especificar um perfil usuário móvel obrigatório, que é um perfil pré-configurado ao qual os usuários não é possível fazer alterações permanentes (as alterações são redefinidas quando o usuário sai), especifique o caminho para o arquivo Ntuser man criado anteriormente, por exemplo, `\\fs1.corp.contoso.com\User Profiles$\default`. Para obter mais informações, consulte [Criar um perfil de usuário obrigatório](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Selecione **OK**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Etapa 7: Opcionalmente, especificar um layout de início para computadores Windows 10

Você pode usar a diretiva de grupo para aplicar um layout específico do menu Iniciar para que os usuários veem o mesmo layout de iniciar em todos os computadores. Se os usuários fazem logon em mais de um PC, e você quiser que ele tenha um layout consistente de início em PCs, certifique-se de que o GPO se aplica a todos os seus PCs.

Para especificar um layout de iniciar, faça o seguinte:

1. Atualizar seus computadores Windows 10 para Windows 10 versão 1607 (também conhecido como a atualização de aniversário) ou mais recente e instalar a atualização cumulativa de 14 de março de 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) ou mais recente.
2. Crie um arquivo completo ou parcial início menu layout XML. Para fazer isso, consulte [personalizar e exportar layout iniciar](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Se você especificar uma *completo* layout de iniciar, um usuário não pode personalizar qualquer parte do menu Iniciar. Se você especificar uma *parcial* layout de iniciar, os usuários podem personalizar tudo, exceto os grupos bloqueados de blocos que você especificar. No entanto, com um layout parcial de início, as personalizações do usuário no menu Iniciar não passarão para outros computadores.
3. Use a diretiva de grupo para aplicar o layout personalizado do início até o GPO que você criou para perfis de usuário móvel. Para fazer isso, consulte [usar a política de grupo para aplicar um layout de início personalizado em um domínio](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Use a diretiva de grupo para definir o valor do registro em seus computadores Windows 10. Para fazer isso, consulte [configurar um Item do registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Ação**   | **Update**                  |
| ------------ | ------------                |
| Hive         | **HKEY_LOCAL_MACHINE**      |
| Caminho da chave     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nome do valor   | **SpecialRoamingOverrideAllowed** |
| Tipo de valor   | **REG_DWORD**               |
| os dados de Valor   | **1** (ou **0** desabilitar) |
| Base         | **Decimal**                 |

5. (Opcional) Habilite otimizações de logon pela primeira vez fazer entrar mais rapidamente para usuários. Para fazer isso, consulte [aplicar políticas para melhorar o tempo de entrada](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. (Opcional) Diminuem ainda mais os tempos de entrada, removendo aplicativos desnecessários da imagem base do Windows 10 que você usar para implantar computadores cliente. 2019 do Windows Server e Windows Server 2016 não tem todos os aplicativos previamente provisionados, portanto, você pode ignorar esta etapa em imagens de servidor.
    - Para remover aplicativos, use o [AppxProvisionedPackage remover](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) cmdlet do Windows PowerShell para desinstalar os aplicativos a seguir. Se seus computadores já implantados pode gerar um script de remoção desses aplicativos usando o [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
      - Microsoft.windowscommunicationsapps\_8wekyb3d8bbwe
      - Microsoft.BingWeather\_8wekyb3d8bbwe
      - Microsoft.DesktopAppInstaller\_8wekyb3d8bbwe
      - Microsoft.Getstarted\_8wekyb3d8bbwe
      - Microsoft.Windows.Photos\_8wekyb3d8bbwe
      - Microsoft.WindowsCamera\_8wekyb3d8bbwe
      - Microsoft.WindowsFeedbackHub\_8wekyb3d8bbwe
      - Microsoft.WindowsStore\_8wekyb3d8bbwe
      - Microsoft.XboxApp\_8wekyb3d8bbwe
      - Microsoft.XboxIdentityProvider\_8wekyb3d8bbwe
      - Microsoft.ZuneMusic\_8wekyb3d8bbwe

>[!NOTE]
>Desinstalando esses aplicativos diminui os tempos de entrada, mas você pode deixá-los instalado se as necessidades de sua implantação de qualquer um deles.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Etapa 8: Habilitar os GPO de perfis de usuário móvel

Se você configurou os Perfis de Usuário Móvel em computadores usando a Política de Grupo ou se você personalizou outras configurações de Perfis de Usuário Móvel usando a Política de Grupo, a próxima etapa será habilitar o GPO, permitindo que ele seja aplicado aos usuários afetados.

>[!TIP]
>Se você planeja implementar o suporte de computador primário, faça isso agora, antes de habilitar o GPO. Isso evita que os dados do usuário sejam copiados para computadores não primários antes de o suporte de computador primário ser habilitado. Para as configurações de política específicas, consulte [implantar computadores primários para redirecionamento de pasta e perfis de usuário móvel](deploy-primary-computers.md).

Aqui está como habilitar o GPO de perfil do usuário de Roaming:

1. Abra Gerenciamento de Política de Grupo.
2. Clique com botão direito no GPO que você criou e, em seguida, selecione **vínculo habilitado**. Uma caixa de seleção é exibida próximo ao item de menu.

## <a name="step-9-test-roaming-user-profiles"></a>Etapa 9: Testar perfis de usuário móvel

Para testar os Perfis de Usuário Móvel, conecte-se a um computador com uma conta de usuário configurada para Perfis de Usuário Móvel ou conecte-se a um computador configurado para Perfis de Usuário Móvel. Em seguida, confirme se o perfil foi redirecionado.

Aqui está como testar perfis de usuário móvel:

1. Conecte-se a um computador primário (caso seu computador primário esteja habilitado para suporte) com uma conta de usuário para a qual você tenha os Perfis de Usuário Móvel habilitados. Se você tiver habilitado os Perfis de Usuário Móvel em computadores específicos, conecte-se a um desses computadores.
2. Caso o usuário tenha sido anteriormente designado para o computador, abra um prompt de comandos com privilégios elevados e, em seguida, digite o comando a seguir para garantir que as configurações de Política de Grupo mais recentes sejam aplicadas ao computador cliente:
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Para confirmar que o perfil do usuário está em roaming, abra **painel de controle**, selecione **sistema e segurança**, selecione **sistema**, selecione **configurações avançadas do sistema** , selecione **configurações** nos perfis de usuário seção e, em seguida, procure **Roaming** no **tipo** coluna.

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Apêndice a: Lista de verificação para a implantação de Perfis de Usuário Móvel

| Status                     | Ação                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Preparar domínio<br>-Ingressar computadores ao domínio<br>-Habilitar o uso de versões separadas de perfil<br>-Criar contas de usuário<br>-(Opcional) implantar redirecionamento de pasta |
| ☐<br><br><br>             | 2. Criar grupo de segurança para Perfis de Usuário Móvel<br>-Nome do grupo de:<br>-Membros: |
| ☐<br><br>                 | 3. Criar um compartilhamento de arquivos para Perfis de Usuário Móvel<br>-Nome do compartilhamento de arquivos: |
| ☐<br><br>                 | 4. Criar um GPO para perfis de usuário móvel<br>-Nome do GPO:|
| ☐                         | 5. Definir configurações de política dos Perfis de Usuário Móvel    |
| ☐<br>☐<br>☐              | 6. Habilitar perfis de usuário móvel<br>-Habilitada no AD DS em contas de usuário?<br>-Habilitado na política de grupo em contas de computador?<br> |
| ☐                         | 7. (Opcional) Especificar um layout obrigatório de início para PCs com Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. (Opcional) Habilitar o suporte de computador primário<br>-Designar computadores primários para usuários<br>-Local do usuário e os mapeamentos de computador primário:<br>-(Opcional) Habilitar suporte a computadores primários para redirecionamento de pasta<br>-Baseadas em computador ou com base no usuário?<br>-(Opcional) Habilitar suporte de computador primário para perfis de usuário móvel |
| ☐                        | 9. Habilitar os GPO de perfis de usuário móvel                |
| ☐                        | 10. Testar perfis de usuário móvel                         |

## <a name="appendix-b-profile-version-reference-information"></a>Apêndice B: Informações de referência de versão de perfil

Cada perfil tem uma versão de perfil que corresponde aproximadamente à versão do Windows na qual o perfil é usado. Por exemplo, Windows 10, versão 1703 e versão 1607 ambos usam o. Versão do perfil v6. A Microsoft cria uma nova versão de perfil somente quando necessário para manter a compatibilidade, o que é por que não todas as versões do Windows inclui uma nova versão de perfil.

A tabela a seguir lista a localização de Perfis de Usuário Móvel em diversas versões do Windows.

| Versão do sistema operacional | Localização do Perfil de Usuário Móvel |
| --- | --- |
| Windows XP e Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista e Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 e Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 e Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3``` (depois que a chave de registro e da atualização de software são aplicadas)<br>```\\<servername>\<fileshare>\<username>.V2``` (antes do software de chave do registro e de atualização são aplicadas) |
| Windows 8.1 e Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4``` (depois que a chave de registro e da atualização de software são aplicadas)<br>```\\<servername>\<fileshare>\<username>.V2``` (antes do software de chave do registro e de atualização são aplicadas) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10, versão 1703 e versão 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Apêndice C: Trabalhar em torno de redefinição de layouts de menu Iniciar após a atualização de

Aqui estão algumas maneiras de contornar obtendo redefinidos após uma atualização in-loco de layouts de menu Iniciar:

- Se apenas um usuário nunca usa o dispositivo e o administrador de TI usa uma estratégia de implantação de sistema operacional gerenciada como o SCCM, eles podem fazer o seguinte:
    
  1. Exportar o layout do menu Iniciar com exportação Startlayout antes da atualização 
  2. Importar o layout do menu Iniciar com a importação StartLayout após o OOBE, mas antes que o usuário faz logon  
 
     > [!NOTE] 
     > Importar uma StartLayout modifica o perfil de usuário padrão. Todos os perfis de usuário criados após a importação terá o Layout de iniciar importado.
 
- Os administradores de TI podem optar por gerenciar o Layout do início com diretiva de grupo. Política de grupo fornece uma solução de gerenciamento centralizado para aplicar um Layout de iniciar padronizado aos usuários. Há 2 modos para os modos usando diretiva de grupo para o gerenciamento de início. Bloqueio completo e parcial bloqueio. O cenário completo de bloqueio impede que o usuário fazer alterações ao layout do início. O cenário de bloqueio parcial permite que o usuário fazer alterações em uma área específica do início. Para obter mais informações, consulte [personalizar e exportar layout iniciar](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > Alterações do usuário feita no cenário de bloqueio parcial ainda serão perdidas durante a atualização.

- Permitir que o início de redefinição do layout ocorrer e permitir que os usuários finais reconfigurar o início. Um email de notificação ou outra notificação pode ser enviada aos usuários finais esperam seus layouts de início para ser redefinido após o sistema operacional atualizar para o menor impacto. 

# <a name="change-history"></a>Histórico de alterações

A tabela a seguir resume algumas das alterações mais importantes para este tópico.

| Date | Descrição |Reason|
| --- | ---         | ---   |
| 1º de maio de 2019 | Atualizações adicionadas para o Windows Server 2019 |
| 10 de abril de 2018 | Discussão adicional sobre quando as personalizações do usuário inicial são perdidas após uma atualização in-loco do sistema operacional|Texto explicativo problema conhecido. |
| 13 de março de 2018 | Atualizado para o Windows Server 2016 | Movido para fora da biblioteca de versões anteriores e atualizado para a versão atual do Windows Server. |
| 13 de abril de 2017 | Adicionadas informações de perfil para Windows 10, versão 1703 e esclareceu o trabalho de versões de perfil móvel como ao atualizar os sistemas operacionais, consulte [considerações ao usar perfis de usuário móvel em várias versões do Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Comentários do cliente. |
| 14 de março de 2017 | Etapa opcional adicionada para especificar um layout obrigatório de início para PCs com Windows 10 no [apêndice a: Lista de verificação para implantação de perfis de usuário móvel](#appendix-a-checklist-for-deploying-roaming-user-profiles). |Alterações de recurso na atualização mais recente do Windows. |
| 23 de janeiro de 2017 | Adicionada uma etapa para [etapa 4: Opcionalmente, criar um GPO para perfis de usuário móvel](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) delegar permissões de leitura para usuários autenticados, que agora é necessária devido a uma atualização de segurança de diretiva de grupo.|Alterações de segurança para processamento de diretiva de grupo. |
| 29 de dezembro de 2016 | Adicionou um link no [etapa 8: Habilitar o GPO de perfis de usuário de Roaming](#step-8-enable-the-roaming-user-profiles-gpo) para torná-lo mais fácil de obter informações sobre como definir a política de grupo para computadores primários. Também corrigidos algumas referências errada as etapas 5 e 6 que tinha os números.|Comentários do cliente. |
| 5 de dezembro de 2016 | Informações adicional explicando um problema de roaming de configurações de menu Iniciar. | Comentários do cliente. |
| 6 de julho de 2016 | Adicionar sufixos de versão de perfil do Windows 10 no [apêndice b: Informações de referência de versão de perfil](#appendix-b-profile-version-reference-information). Também removido o Windows XP e Windows Server 2003 na lista de sistemas operacionais com suporte. | Atualizações para as novas versões do Windows e removida informações sobre as versões do Windows que não têm mais suporte. |
| 7 de julho de 2015 | Adicionado o requisito e a etapa para desabilitar a disponibilidade contínua ao usar um servidor de arquivos em cluster. | Compartilhamentos de arquivos em cluster têm desempenho melhor para pequenas gravações (que são típicas com perfis de usuário móvel) quando a disponibilidade contínua está desabilitada. |
| 19 de março de 2014 | Sufixos de versão de perfil em letras maiusculas (. V2. V3. V4) no [apêndice b: Informações de referência de versão de perfil](#appendix-b-profile-version-reference-information). | Embora o Windows diferencia maiusculas de minúsculas, se você usar NFS com o compartilhamento de arquivos, é importante ter as maiusculas/minúsculas (letras maiusculas) correta para o sufixo do perfil. |
| 9 de outubro de 2013 | Revisado para Windows Server 2012 R2 e Windows 8.1, esclarecidos alguns pontos e adicionado a [considerações ao usar perfis de usuário móvel em várias versões do Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) e [apêndice b: Informações de referência de versão de perfil](#appendix-b-profile-version-reference-information) seções. | Atualizações para a nova versão; comentários do cliente. |

## <a name="more-information"></a>Mais informações

- [Implantar o redirecionamento de pasta, arquivos Offline e perfis de usuário móvel](deploy-folder-redirection.md)
- [Implantar computadores primários para redirecionamento de pasta e perfis de usuário móvel](deploy-primary-computers.md)
- [Implementando o gerenciamento de estado do usuário](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Instrução de suporte da Microsoft em torno de dados de perfil de usuário duplicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Sideload de aplicativos com o DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Solução de problemas de empacotamento, implantação e consulta de aplicativos baseados em tempo de execução do Windows](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)