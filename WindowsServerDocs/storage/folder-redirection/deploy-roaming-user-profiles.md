---
title: Como implantar Perfis de Usuários Móveis
TOCTitle: Deploying Roaming User Profiles
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.date: 06/07/2019
ms.author: jgerend
ms.openlocfilehash: 8feed2adb606edfb6068d7fe10c18baf142077ac
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "76822339"
---
# <a name="deploying-roaming-user-profiles"></a>Como implantar Perfis de Usuários Móveis

>Aplica-se a: Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2019, Windows Server 2016, Windows Server (Canal Semestral), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Este tópico descreve como usar o Windows Server para implantar os [Perfis de Usuários Móveis](folder-redirection-rup-overview.md) para os computadores cliente do Windows. Os Perfis de Usuários Móveis redirecionam os perfis de usuário para um compartilhamento de arquivo para que os usuários recebam as mesmas configurações de sistema operacional e de aplicativos em vários computadores.

Para obter uma lista das alterações recentes deste tópico, confira a seção [Histórico de alterações](#change-history) deste tópico.

> [!IMPORTANT]
> Devido às alterações de segurança feitas no [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), atualizamos a [Etapa 4: Opcionalmente, crie um GPO para Perfis de Usuários Móveis](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) neste tópico para que o Windows possa aplicar corretamente a política de Perfis de Usuários Móveis (e não reverter para políticas locais nos PCs afetados).

> [!IMPORTANT]
>  As personalizações do usuário para o menu Iniciar são perdidas após uma atualização in-loco do sistema operacional na seguinte configuração:
> - Os usuários estão configurados para um perfil móvel
> - Os usuários têm permissão para fazer alterações no menu Iniciar
>
> Como resultado, o menu Iniciar é redefinido para o padrão da nova versão do sistema operacional após a atualização in-loco do sistema operacional. Para obter soluções alternativas, confira [Apêndice C: Solução alternativa para a redefinição de layouts do menu Iniciar após atualizações](#appendix-c-working-around-reset-start-menu-layouts-after-upgrades).

## <a name="prerequisites"></a>Pré-requisitos

### <a name="hardware-requirements"></a>Requisitos de hardware

Os Perfis de Usuários Móveis requerem um computador baseado em x64 ou x86. Eles não são compatíveis com o Windows RT.

### <a name="software-requirements"></a>Requisitos de software

Os Perfis de Usuário Móvel possuem os seguintes requisitos de software:

- Se estiver implantando os Perfis de Usuário Móvel com Redirecionamento de Pasta em um ambiente com perfis de usuários locais existentes, implante o Redirecionamento de Pasta antes dos Perfis de Usuário Móvel para minimizar o tamanho dos perfis móveis. Após as pastas de usuários existentes serem redirecionadas com êxito, você pode implantar os Perfis de Usuário Móvel.
- Para administrar os Perfis de Usuário Móvel, você deve ser registrado como um membro do grupo de segurança Administradores de Domínio, o grupo de segurança Administradores Corporativos ou o grupo de segurança Proprietários Criadores de Política de Grupo.
- Os computadores cliente devem executar o Windows 10, o Windows 8.1, o Windows 8, o Windows 7, o Windows Vista, o Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2008 R2 ou o Windows Server 2008.
- Os computadores cliente devem ser integrados aos AD DS (Serviços de Domínio Active Directory) que você está gerenciando.
- Um computador deve ser disponibilizado com o Gerenciamento de Política de Grupo e o Centro de Administração do Active Directory instalados.
- Um servidor de arquivos deve estar disponível para hospedar perfis de usuário móvel.

    - Se o compartilhamento de arquivos usar os Namespaces do DFS, as pastas DFS (links) deverão ter um único destino para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Se o compartilhamento de arquivos usar a Replicação do DFS para replicar o conteúdo com outro servidor, os usuários devem poder acessar apenas o servidor de origem para evitar que os usuários façam edições conflitantes em diferentes servidores.
    - Se o compartilhamento de arquivos for clusterizado, desabilite a disponibilidade contínua no compartilhamento de arquivos para evitar problemas de desempenho.
- Para usar o suporte de computador primário nos Perfis de Usuários Móveis, há requisitos adicionais do computador cliente e do esquema do Active Directory. Para obter mais informações, Confira [Implantar computadores primários para Redirecionamento de Pastas e Perfis de Usuários Móveis](deploy-primary-computers.md).
- O layout do menu Iniciar de um usuário não será transferido no Windows 10, Windows Server 2019 ou Windows Server 2016 se eles estiverem usando mais de um PC, Host da Sessão da Área de Trabalho Remota ou servidor de VDI (Virtualized Desktop Infrastructure). Como alternativa, você pode especificar um layout do menu Iniciar conforme descrito neste tópico. Ou você pode usar discos de perfil do usuário, que transferem adequadamente as configurações do menu Iniciar quando usados com servidores Host da Sessão da Área de Trabalho Remota ou servidores de VDI. Para obter mais informações, confira [Gerenciamento de dados de usuário mais fácil com discos de perfil do usuário no Windows Server 2012](https://blogs.technet.microsoft.com/enterprisemobility/2012/11/13/easier-user-data-management-with-user-profile-disks-in-windows-server-2012/).

### <a name="considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows"></a>Considerações ao usar os Perfis de Usuário Móvel em diversas versões do Windows

Se você decidir usar os Perfis de Usuário Móvel em diferentes versões do Windows, recomendamos executar as seguintes ações:

- Configure o Windows para manter versões de perfil separadas para cada versão do sistema operacional. Isso ajuda a prevenir problemas indesejáveis e imprevisíveis, como perfis corrompidos.
- Use o Redirecionamento de Pastas para armazenar arquivos de usuários, como documentos e imagens fora dos perfis de usuário. Isso permite que os mesmos arquivos estejam disponíveis aos usuários nas versões do sistema operacional. Isso também mantém os perfis menores e a conexão mais rápida.
- Alocar armazenamento suficiente para os Perfis de Usuário Móvel. Se você der suporte a duas versões do sistema operacional, os perfis dobrarão em número (e, portanto, o espaço total consumido), pois um perfil separado é mantido para cada versão do sistema operacional.
- Não use os Perfis de Usuários Móveis em computadores que usam o Windows Vista/Windows Server 2008 e o Windows 7/Windows Server 2008 R2. A transferência entre essas versões do sistema operacional não é compatível devido a incompatibilidades nas versões de perfil.
- Informe aos seus usuários que as alterações feitas em uma versão do sistema operacional não passarão para outra versão do sistema operacional.
- Ao mover seu ambiente para uma versão do Windows que usa uma versão de perfil diferente (como do Windows 10 para o Windows 10, versão 1607, confira [Apêndice B: Informações de referência da versão do perfil](#appendix-b-profile-version-reference-information) para uma lista), os usuários recebem um perfil de usuário móvel novo e vazio. Você pode minimizar o impacto de obter um novo perfil usando o Redirecionamento de Pastas para redirecionar pastas comuns. Não existe um método compatível de migração de perfis de usuários móveis de uma versão do perfil para outra.

## <a name="step-1-enable-the-use-of-separate-profile-versions"></a>Etapa 1: Habilitar o uso de versões separadas de perfil

Se você estiver implantando Perfis de Usuários Móveis em computadores que usam o Windows 8.1, o Windows 8, o Windows Server 2012 R2 ou o Windows Server 2012, recomendamos fazer algumas alterações no ambiente Windows antes da implantação. Essas alterações ajudam a garantir que as atualizações futuras do sistema operacional sejam suaves, o que facilita a capacidade de executar simultaneamente diversas versões do Windows com Perfis de Usuário Móvel.

Para fazer essas alterações, use o procedimento a seguir.

1. Baixe e instale a atualização de software apropriada em todos os computadores nos quais usará perfis móveis, obrigatórios, superobrigatórios ou padrão de domínio:

    - Windows 8.1 ou o Windows Server 2012 R2: instale a atualização de software descrita no artigo [2887595](https://support.microsoft.com/kb/2887595) na Base de Dados de Conhecimento Microsoft (quando lançado).
    - Windows 8 ou o Windows Server 2012: instale a atualização de software descrita no artigo [2887239](https://support.microsoft.com/kb/2887239) na Base de Dados de Conhecimento Microsoft.

2. Em todos os computadores que usam o Windows 8.1, o Windows 8, o Windows Server 2012 R2 ou o Windows Server 2012 em que você usará os Perfis de Usuários Móveis, use o Editor do Registro ou a Política de Grupo para criar o valor DWORD de chave do Registro a seguir e defini-lo como `1`. Para obter informações sobre como criar chaves do registro usando a política de grupo, consulte [Configurar um Item do Registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

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

Veja como criar um grupo de segurança para os Perfis de Usuários Móveis:

1. Abra o Gerenciador do Servidor em um computador com o Centro de Administração do Active Directory instalado.
2. No menu **Ferramentas**, selecione **Centro de Administração do Active Directory**. O Centro de Administração do Active Directory é exibido.
3. Clique com o botão direito do mouse no domínio ou unidade organizacional apropriado, selecione **Novo** e, em seguida, selecione **Grupo**.
4. Na janela **Criar Grupo** , na seção **Grupo** , especifique as seguintes configurações:

    - Em **Nome do grupo**, digite o nome do grupo de segurança, por exemplo: **Computadores e Usuários de Perfis de Usuário Móvel**.
    - Em **Escopo do grupo**, selecione **Segurança** e, em seguida, selecione **Global**.

5. Na seção **Membros**, selecione **Adicionar**. A caixa de diálogo Selecionar Usuários, Contatos, Computadores, Contas de Serviço ou Grupos é exibida.
6. Se desejar incluir contas de computador ao grupo de segurança, selecione **Tipos de Objetos**, marque a caixa de seleção **Computadores** e selecione **OK**.
7. Digite os nomes dos usuários, grupos e/ou computadores para os quais deseja implantar Perfis de Usuários Móveis, selecione **OK** e, depois, **OK** novamente.

## <a name="step-3-create-a-file-share-for-roaming-user-profiles"></a>Etapa 3: Criar um compartilhamento de arquivos para perfis de usuário móvel

Se você ainda não tiver um compartilhamento de arquivo separado para os perfis de usuários móveis (independente de outros compartilhamentos para pastas redirecionadas para evitar armazenamento em cache inadvertido da pasta de perfil móvel), use o procedimento a seguir para criar um compartilhamento de arquivo em um servidor que use o Windows Server.

> [!NOTE]
> Algumas funcionalidades podem ser diferentes ou não estar disponíveis dependendo da versão do Windows Server que você está usando.

Veja como criar um compartilhamento de arquivo no Windows Server:

1. No painel de navegação Gerenciador do Servidor, selecione **Serviços de Arquivos e Armazenamento**e, em seguida, selecione **Compartilhamentos** para exibir a página Compartilhamentos.
2. No bloco Compartilhamentos, selecione **Tarefas** e, em seguida, selecione **Novo Compartilhamento**. O Assistente Novo Compartilhamento é exibido.
3. Na página **Selecionar Perfil**, selecione **Compartilhamento SMB – Rápido**. Se você tiver o Gerenciador de Recursos de Servidor de Arquivos instalado e estiver usando propriedades de gerenciamento de pasta, em vez disso, selecione **Compartilhamento SMB – Avançado**.
4. Na página **Compartilhar Local** , selecione o servidor e o volume nos quais deseja criar o compartilhamento.
5. Na página **Compartilhar Nome** , digite um nome para o compartilhamento (por exemplo, **User Profiles$** ) na caixa **Compartilhar nome** .

    > [!TIP]
    > Ao criar o compartilhamento, oculte-o colocando um ```$``` após o nome do compartilhamento. Isso oculta o compartilhamento de navegadores casuais.

6. Na página **Outras Configurações** , desmarque a caixa de seleção **Habilitar disponibilidade contínua** , se presente, e, opcionalmente, marque as caixas de seleção **Habilitar enumeração baseada em acesso** e **Criptografar o acesso a dados** .
7. Na página **Permissões**, clique em **Personalizar Permissões…** . A caixa de diálogo Configurações de Segurança Avançada é exibida.
8. Clique em **Desabilitar herança** e, em seguida, selecione **Converter permissões herdadas em permissões explícitas nesse objeto**.
9. Defina as permissões conforme descrito em [Permissões necessárias para o compartilhamento de arquivo que hospeda perfis de usuários móveis](#required-permissions-for-the-file-share-hosting-roaming-user-profiles) e mostrado na captura de tela a seguir, removendo as permissões para contas e grupos não listados e incluindo permissões especiais ao grupo Computadores e Usuários de Perfis de Usuários Móveis, criado por você na Etapa 1.
    
    ![Janela Configurações de Segurança Avançadas mostrando permissões conforme descrito na Tabela 1](media/advanced-security-user-profiles.jpg)
    
    **Figura 1** Definir as permissões para o compartilhamento de perfis de usuário móvel
10. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Propriedades de Gerenciamento** , selecione o valor de Uso da Pasta de **Arquivos do Usuário** .
11. Se você escolher o perfil **Compartilhamento SMB - Avançado** , na página **Cota** , opcionalmente selecione uma cota para aplicar aos usuários do compartilhamento.
12. Na página **Confirmação**, selecione **Criar**.

### <a name="required-permissions-for-the-file-share-hosting-roaming-user-profiles"></a>Permissões necessárias para o compartilhamento de arquivo que hospeda perfis de usuários móveis

| Conta de Usuário | Acesso | Aplica-se a |
|   -   |   -   |   -   |
|   Sistema    |  Controle total     |  Essa pasta, subpastas e arquivos     |
|  Administradores     |  Controle total     |  Apenas essa pasta     |
|  Criador/Proprietário     |  Controle total     |  Apenas subpastas e arquivos     |
| Grupo de segurança de usuários que precisam colocar dados no compartilhamento (Computadores e Usuários de Perfis de Usuário Móvel)      |  Listar pasta/ler dados *(permissões avançadas)* <br />Criar pastas/acrescentar dados *(permissões avançadas)* |  Apenas essa pasta     |
| Outros grupos e contas   |  Nenhum (remover)     |       |

## <a name="step-4-optionally-create-a-gpo-for-roaming-user-profiles"></a>Etapa 4: Opcionalmente, criar um GPO para Perfis de Usuário Móvel

Se você ainda não possui um GPO criado para as configurações de Perfis de Usuário Móvel, use o procedimento a seguir para criar um GPO vazio para ser usado com os Perfis de Usuário Móvel. Esse GPO permite a você definir configurações de Perfis de Usuário Móvel (como suporte ao computador primário, que é discutido separadamente) e também pode ser usado para habilitar os Perfis de Usuário Móvel em computadores, como é feito geralmente ao implantar em ambientes de área de trabalho virtualizada ou com Serviços de Área de Trabalho Remota.

Veja como criar um GPO para Perfis de Usuários Móveis:

1. Abra o Gerenciador do Servidor em um computador com o Gerenciamento de Política de Grupo instalado.
2. No menu **Ferramentas**, selecione **Gerenciamento de Política de Grupo**. O Gerenciamento de Política de Grupo é exibido.
3. Clique com o botão direito do mouse no domínio ou UO em que deseja instalar os Perfis de Usuários Móveis e selecione **Criar um GPO neste domínio e fornecer um link para ele aqui**.
4. Na caixa de diálogo **Novo GPO**, digite um nome para o GPO (por exemplo, **Configurações de Perfil de Usuário Móvel**), e selecione **OK**.
5. Clique com o botão direito do mouse no GPO recentemente criado e, em seguida, desmarque a caixa de seleção **Vínculo habilitado** . Isso evita que o GPO seja aplicado até que você finalize a configuração dele.
6. Selecione o GPO. Na seção **Filtragem de Segurança** da guia **Escopo**, selecione **Usuários Autenticados** e, em seguida, selecione **Remover** para impedir que o GPO seja aplicado a todos.
7. Na seção **Filtro de Segurança**, selecione **Adicionar**.
8. Na caixa de diálogo **Selecionar Usuário, Computador ou Grupo**, digite o nome do grupo de segurança criado por você na Etapa 1 (por exemplo, **Computadores e Usuários de Perfis de Usuários Móveis**) e selecione **OK**.
9. Selecione a guia **Delegação**, selecione **Adicionar**, insira **Usuários Autenticados**, selecione **OK** e, em seguida, selecione **OK** novamente para aceitar as permissões de leitura padrão.
    
    Esta etapa é necessária devido a alterações de segurança feitas em [MS16-072](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016).

>[!IMPORTANT]
>Devido às alterações de segurança feitas em [MS16-072A](https://support.microsoft.com/help/3163622/ms16-072-security-update-for-group-policy-june-14%2c-2016), agora você deve conceder ao grupo usuários autenticados permissões de leitura delegadas para o GPO. Caso contrário, o GPO não será aplicado aos usuários ou, se já estiver aplicado, será removido, redirecionando os perfis de usuário de volta para o computador local. Para obter mais informações, confira [Implantar a Atualização de Segurança da Política de Grupo MS16-072](https://blogs.technet.microsoft.com/askds/2016/06/22/deploying-group-policy-security-update-ms16-072-kb3163622/).

## <a name="step-5-optionally-set-up-roaming-user-profiles-on-user-accounts"></a>Etapa 5: Opcionalmente, configurar perfis de usuário móvel em contas de usuários

Se você estiver implantando os Perfis de Usuário Móvel nas contas de usuários, use o procedimento a seguir para especificar os perfis de usuário móvel para as contas de usuário nos Serviços de Domínio Active Directory. Se você estiver implantando os Perfis de Usuários Móveis aos computadores, como normalmente é feito para as implantações de Serviços de Área de Trabalho Remota ou de área de trabalho virtualizada, use, em vez disso, o procedimento documentado na [Etapa 6: Opcionalmente, configurar Perfis de Usuários Móveis em computadores](#step-6-optionally-set-up-roaming-user-profiles-on-computers).

> [!NOTE]
> Se você configurar os Perfis de Usuário Móvel em contas de usuário usando o Active Directory e em computadores usando a Política de Grupo, a configuração baseada em computadores terá precedência.

Veja como instalar os Perfis de Usuários Móveis em contas de usuário:

1. No Centro de Administração do Active Directory, navegue até o contêiner **Usuários** (ou UO) no domínio apropriado.
2. Selecione todos os usuários para os quais deseja designar um perfil de usuário móvel, clique com o botão direito do mouse nos usuários e selecione **Propriedades**.
3. Na seção **Perfil**, selecione a caixa de seleção **Caminho de perfil:** e insira o caminho para o compartilhamento de arquivo no qual deseja armazenar o perfil de usuário móvel do usuário, seguido por `%username%` (que é automaticamente substituído pelo nome de usuário na primeira vez em que o usuário entrar). Por exemplo:
    
    `\\fs1.corp.contoso.com\User Profiles$\%username%`
    
    Para especificar um perfil de usuário móvel obrigatório, especifique o caminho para o arquivo NTuser.man criado por você anteriormente, por exemplo, `fs1.corp.contoso.comUser Profiles$default`. Para obter mais informações, confira [Criar perfis de usuário obrigatórios](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
4. Selecione **OK**.

> [!NOTE]
> Por padrão, a implantação de todos os aplicativos com base no Runtime do Windows® (Windows Store) é permitida ao usar os Perfis de Usuário Móvel. No entanto, ao usar um perfil especial, os aplicativos não são implantados por padrão. Os perfis especiais são perfis de usuários nos quais as alterações são descartadas após o usuário se registrar:
> <br><br>Para remover as restrições na implantação do aplicativo para perfis especiais, habilite a configuração de política **Allow deployment operations in special profiles** (localizada em Computer Configuration\Policies\Administrative Templates\Windows Components\App Package Deployment). No entanto, os aplicativos implantados nesse cenário deixarão alguns dados armazenados no computador, que poderia criar acúmulos, por exemplo, se houvesse centenas de usuários em um único computador. Para limpar os aplicativos, localize ou desenvolva uma ferramenta que use a API [CleanupPackageForUserAsync](https://msdn.microsoft.com/library/windows/apps/windows.management.deployment.packagemanager.cleanuppackageforuserasync.aspx) para limpar pacotes de aplicativos para usuários que não tenham um perfil no computador.
> <br><br>Para mais informações em segundo plano sobre os aplicativos da Windows Store, consulte [Gerenciar o acesso de clientes à Windows Store](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh832040(v=ws.11)>).

## <a name="step-6-optionally-set-up-roaming-user-profiles-on-computers"></a>Etapa 6: Opcionalmente, configurar perfis de usuário móvel em computadores

Se estiver implantando os Perfis de Usuário Móvel para computadores, como normalmente é feito para as implantações de Serviços de área de trabalho remota ou de área de trabalho virtualizada, use o procedimento a seguir. Se estiver implantando Perfis de Usuários Móveis para as contas de usuário, use o procedimento descrito na [Etapa 5: Opcionalmente, configurar Perfis de Usuários Móveis em contas de usuários](#step-5-optionally-set-up-roaming-user-profiles-on-user-accounts).

É possível usar a Política de Grupo para aplicar Perfis de Usuários Móveis para computadores que executam o Windows 8.1, o Windows 8, o Windows 7, o Windows Vista, o Windows Server 2019, o Windows Server 2016, o Windows Server 2012 R2, o Windows Server 2012, o Windows Server 2008 R2 ou o Windows Server 2008.

> [!NOTE]
> Se você configurar Perfis de Usuário Móvel em computadores usando a Política de Grupo e em contas de usuários usando o Active Directory, a configuração de políticas com base em computadores terá precedência.

Veja como configurar Perfis de Usuários Móveis em computadores:

1. Abra o Gerenciador do Servidor em um computador com o Gerenciamento de Política de Grupo instalado.
2. No menu **Ferramentas**, selecione **Gerenciamento de Política de Grupo**. O Gerenciamento de Política de Grupo será exibido.
3. No Gerenciamento de Política de Grupo, clique com o botão direito do mouse no GPO criado por você na Etapa 3 (por exemplo, **Configurações de Perfis de Usuários Móveis**) e selecione **Editar**.
4. Na janela Editor de Gerenciamento de Política de Grupo, navegue até **Configuração do Computador**, em seguida, **Políticas**, **Modelos Administrativos**, **Sistema**e **Perfis de Usuário**.
5. Clique com o botão direito do mouse em **Definir caminho de perfil móvel para todos os usuários registrados em log neste computador** e selecione **Editar**.
    > [!TIP]
    > Uma pasta base do usuário, se configurada, é a pasta padrão usada por alguns programas como o Windows PowerShell. É possível configurar um local alternativo ou local de rede por usuário usando a seção **Pasta base** das propriedades de conta do usuário em AD DS. Para configurar a localização da pasta base para todos os usuários em um computador que usa o Windows 8.1, o Windows 8, o Windows Server 2019, o Windows Server 2016, o Windows Server 2012 R2 ou o Windows Server 2012 em um ambiente de área de trabalho virtual, habilite a configuração de política **Definir pasta base de usuário** e especifique o compartilhamento de arquivo e a letra da unidade para mapear (ou especifique uma pasta local). Não use elipses ou variáveis de ambiente. O alias do usuário é anexado ao final do caminho especificado durante o registro do usuário.
6. Na caixa de diálogo **Propriedades** selecione **Habilitado**
7. Na caixa **Usuários registrados em log neste computador devem usar este caminho de perfil móvel**, insira o caminho para o compartilhamento de arquivo que deseja armazenar o perfil de usuário móvel do usuário, seguido por `%username%` (que é substituído automaticamente pelo nome de usuário na primeira vez em que ele entrar). Por exemplo:

    `\\fs1.corp.contoso.com\User Profiles$\%username%`

    Para especificar um perfil de usuário móvel obrigatório, que é um perfil pré-configurado ao qual os usuários não podem fazer alterações permanentes (as alterações são redefinidas quando o usuário sai), especifique o caminho para o arquivo NTuser.man criado por você anteriormente, por exemplo, `\\fs1.corp.contoso.com\User Profiles$\default`. Para obter mais informações, consulte [Criar um perfil de usuário obrigatório](https://docs.microsoft.com/windows/client-management/mandatory-user-profile).
8. Selecione **OK**.

## <a name="step-7-optionally-specify-a-start-layout-for-windows-10-pcs"></a>Etapa 7: Opcionalmente, especifique um layout do menu Iniciar para PCs com Windows 10

Você pode usar Política de Grupo para aplicar um layout de menu Iniciar específico para que os usuários vejam o mesmo layout do menu Iniciar em todos os PCs. Se os usuários entrarem em mais de um computador e você quiser que eles tenham um layout de menu Iniciar entre PCs, certifique-se de que o GPO se aplique a todos os seus computadores.

Para especificar um layout de menu Iniciar, faça o seguinte:

1. Atualize seus PCs com Windows 10 para o Windows 10 versão 1607 (também conhecida como atualização de aniversário) ou mais recente e instale a atualização cumulativa de 14 de março de 2017 ([KB4013429](https://support.microsoft.com/kb/4013429)) ou mais recente.
2. Crie um arquivo XML completo ou parcial de layout do menu Iniciar. Para fazer isso, confira [Personalizar e exportar o layout do menu Iniciar](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
    * Se você especificar um layout do menu Iniciar *completo*, os usuários não poderão personalizar nenhuma parte do menu Iniciar. Se você especificar um layout do menu Iniciar *parcial*, os usuários poderão personalizar tudo, exceto os grupos de blocos bloqueados especificados. No entanto, com um layout do menu Iniciar parcial, as personalizações do menu Iniciar não serão transferidas para outros PCs.
3. Use a Política de Grupo para aplicar o layout do menu Iniciar personalizado ao GPO que você criou para Perfis de Usuários Móveis. Para fazer isso, confira [Usar a Política de Grupo para aplicar um layout de tela inicial personalizado em um domínio](https://docs.microsoft.com/windows/configuration/customize-windows-10-start-screens-by-using-group-policy#bkmk-domaingpodeployment).
4. Use Política de Grupo para definir o valor do Registro a seguir em seus PCs com Windows 10. Para fazer isso, confira [Configurar um item do Registro](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc753092(v=ws.11)>).

| **Ação**   | **Atualização**                  |
| ------------ | ------------                |
| Hive         | **HKEY_LOCAL_MACHINE**      |
| Caminho da chave     | **Software\Microsoft\Windows\CurrentVersion\Explorer** |
| Nome do valor   | **SpecialRoamingOverrideAllowed** |
| Tipo de valor   | **REG_DWORD**               |
| os dados de Valor   | **1** (ou **0** para desabilitar) |
| Base         | **Decimal**                 |

5. (Opcional) Habilite otimizações de primeiro logon para tornar a entrada mais rápida para os usuários. Para fazer isso, confira [Aplicar políticas para aprimorar o tempo de entrada](https://docs.microsoft.com/windows/client-management/mandatory-user-profile#apply-policies-to-improve-sign-in-time).
6. (Opcional) Diminua ainda mais os tempos de entrada removendo aplicativos desnecessários da imagem base do Windows 10 que você usa para implantar computadores cliente. O Windows Server 2019 e o Windows Server 2016 não têm nenhum aplicativo previamente provisionado, portanto, você pode ignorar essa etapa em imagens do servidor.
    - Para remover aplicativos, use o cmdlet [Remove-AppxProvisionedPackage](https://docs.microsoft.com/powershell/module/dism/remove-appxprovisionedpackage?view=win10-ps) no Windows PowerShell para desinstalar os aplicativos a seguir. Se os computadores já estiverem implantados, você poderá gerar scripts para a remoção desses aplicativos usando o [Remove-AppxPackage](https://docs.microsoft.com/powershell/module/appx/remove-appxpackage?view=win10-ps).
    
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
>A desinstalação desses aplicativos diminui os tempos de entrada, mas você poderá deixá-los instalados se a implantação precisar de qualquer um deles.

## <a name="step-8-enable-the-roaming-user-profiles-gpo"></a>Etapa 8: Habilitar os GPO de perfis de usuário móvel

Se você configurou os Perfis de Usuário Móvel em computadores usando a Política de Grupo ou se você personalizou outras configurações de Perfis de Usuário Móvel usando a Política de Grupo, a próxima etapa será habilitar o GPO, permitindo que ele seja aplicado aos usuários afetados.

>[!TIP]
>Se planejar implantar suporte de computador primário, faça isso agora, antes de habilitar o GPO. Isso evita que os dados do usuário sejam copiados para computadores não primários antes de o suporte de computador primário ser habilitado. Para encontrar as configurações de política específicas, confira [Implantar computadores primários para Redirecionamento de Pastas e Perfis de Usuários Móveis](deploy-primary-computers.md).

Veja como habilitar o GPO de perfil de usuário móvel:

1. Abra Gerenciamento de Política de Grupo.
2. Clique com o botão direito do mouse no GPO criado por você e selecione **Vínculo Habilitado**. Uma caixa de seleção é exibida próximo ao item de menu.

## <a name="step-9-test-roaming-user-profiles"></a>Etapa 9: Testar perfis de usuário móvel

Para testar os Perfis de Usuário Móvel, conecte-se a um computador com uma conta de usuário configurada para Perfis de Usuário Móvel ou conecte-se a um computador configurado para Perfis de Usuário Móvel. Em seguida, confirme se o perfil foi redirecionado.

Veja como testar os Perfis de Usuários Móveis:

1. Conecte-se a um computador primário (caso seu computador primário esteja habilitado para suporte) com uma conta de usuário para a qual você tenha os Perfis de Usuário Móvel habilitados. Se você tiver habilitado os Perfis de Usuário Móvel em computadores específicos, conecte-se a um desses computadores.
2. Caso o usuário tenha sido anteriormente designado para o computador, abra um prompt de comandos com privilégios elevados e, em seguida, digite o comando a seguir para garantir que as configurações de Política de Grupo mais recentes sejam aplicadas ao computador cliente:
    
    ```PowerShell
    GpUpdate /Force
    ```
3. Para confirmar que o perfil do usuário é móvel, abra o **Painel de Controle**, selecione **Sistema e Segurança**, **Sistema**, **Configurações avançadas do sistema**, **Configurações** na seção Perfis do Usuário e procure por **Móvel** na coluna **Tipo**.

## <a name="appendix-a-checklist-for-deploying-roaming-user-profiles"></a>Apêndice A: Lista de verificação para a implantação de Perfis de Usuário Móvel

| Status                     | Ação                                                |
| ---                        | ------                                                |
| ☐<br>☐<br>☐<br>☐<br>☐   | 1. Preparar domínio<br>– Ingressar computadores no domínio<br>– Habilitar o uso de versões separadas de perfil<br>– Criar contas de usuário<br>– (Opcional) Implantar o Redirecionamento de Pastas |
| ☐<br><br><br>             | 2. Criar grupo de segurança para Perfis de Usuário Móvel<br>– Nome do grupo:<br>– Membros: |
| ☐<br><br>                 | 3. Criar um compartilhamento de arquivos para Perfis de Usuário Móvel<br>– Nome do compartilhamento de arquivo: |
| ☐<br><br>                 | 4. Criar um GPO para perfis de usuário móvel<br>– Nome do GPO:|
| ☐                         | 5. Definir configurações de política dos Perfis de Usuário Móvel    |
| ☐<br>☐<br>☐              | 6. Habilitar Perfis de Usuários Móveis<br>– Habilitados no AD DS em contas de usuário?<br>– Habilitados na Política de Grupo em contas de computador?<br> |
| ☐                         | 7. (Opcional) Especificar um layout do menu Iniciar para PCs com Windows 10 |
| ☐<br>☐<br><br>☐<br><br>☐ | 8. (Opcional) Habilitar suporte ao computador primário<br>– Designar computadores primários para usuários<br>– Localização dos mapeamentos de computador primário e usuário:<br>– (Opcional) Habilitar suporte de computador primário para redirecionamento de pastas<br>– Baseado no computador ou no usuário?<br>– (Opcional) Habilitar suporte de computador primário para Perfis de Usuários Móveis |
| ☐                        | 9. Habilitar os GPO de perfis de usuário móvel                |
| ☐                        | 10. Testar perfis de usuário móvel                         |

## <a name="appendix-b-profile-version-reference-information"></a>Apêndice B: Informações de referência de versão de perfil

Cada perfil tem uma versão de perfil que corresponde aproximadamente à versão do Windows na qual ele é usado. Por exemplo, as versões 1703 e 1607 do Windows 10 usam a versão do perfil .V6. A Microsoft cria uma versão de perfil somente quando necessário para manter a compatibilidade, motivo pelo qual nem todas as versões do Windows incluem uma nova versão de perfil.

A tabela a seguir lista a localização de Perfis de Usuário Móvel em diversas versões do Windows.

| Versão do sistema operacional | Localização do Perfil de Usuário Móvel |
| --- | --- |
| Windows XP e Windows Server 2003 | ```\\<servername>\<fileshare>\<username>``` |
| Windows Vista e Windows Server 2008 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 7 e Windows Server 2008 R2 | ```\\<servername>\<fileshare>\<username>.V2``` |
| Windows 8 e Windows Server 2012 | ```\\<servername>\<fileshare>\<username>.V3``` (após a atualização de software e a chave do Registro terem sido aplicadas)<br>```\\<servername>\<fileshare>\<username>.V2``` (antes de a atualização de software e a chave do Registro terem sido aplicadas) |
| Windows 8.1 e Windows Server 2012 R2 | ```\\<servername>\<fileshare>\<username>.V4``` (após a atualização de software e a chave do Registro terem sido aplicadas)<br>```\\<servername>\<fileshare>\<username>.V2``` (antes de a atualização de software e a chave do Registro terem sido aplicadas) |
| Windows 10 | ```\\<servername>\<fileshare>\<username>.V5``` |
| Windows 10, versões 1703 e 1607 | ```\\<servername>\<fileshare>\<username>.V6``` |

## <a name="appendix-c-working-around-reset-start-menu-layouts-after-upgrades"></a>Apêndice C: Solução alternativa para a redefinição de layouts do menu Iniciar após atualizações

Aqui estão algumas maneiras de contornar o problema de os layouts do menu Iniciar serem redefinidos após uma atualização in-loco:

- Se apenas um usuário usar o dispositivo e o administrador de TI usar uma estratégia de implantação de sistema operacional gerenciada, como Configuration Manager, eles poderão fazer o seguinte:
    
  1. Exportar o layout do menu Iniciar com Export-Startlayout antes da atualização 
  2. Importar o layout do menu Iniciar com Import-StartLayout após o OOBE, mas antes de o usuário entrar  
 
     > [!NOTE] 
     > A importação de um StartLayout modifica o perfil do usuário padrão. Todos os perfis de usuário criados após a importação receberão o layout do menu Iniciar importado.
 
- Os administradores de TI podem optar por gerenciar o layout do menu Iniciar com a Política de Grupo. O uso da Política de Grupo fornece uma solução de gerenciamento centralizada para aplicar um layout de menu Iniciar padronizado aos usuários. Há dois modos de usar Política de Grupo para o gerenciamento do menu Iniciar. Bloqueio completo e bloqueio parcial. O cenário de bloqueio completo impede que o usuário faça qualquer alteração no layout do menu Iniciar. O cenário de bloqueio parcial permite que o usuário faça alterações em uma área específica do menu Iniciar. Para obter mais informações, confira [Personalizar e exportar o layout do menu Iniciar](https://docs.microsoft.com/windows/configuration/customize-and-export-start-layout).
        
   > [!NOTE]
   > As alterações feitas pelo usuário no cenário de bloqueio parcial ainda serão perdidas durante a atualização.

- Permita que a redefinição do layout do menu Iniciar ocorra e permita que os usuários finais o reconfigurem. Um email de notificação ou outra notificação pode ser enviada aos usuários finais para esperar que seus layouts do menu Iniciar sejam redefinidos após a atualização do sistema operacional para minimizar o impacto. 

## <a name="change-history"></a>Histórico de alterações

A tabela a seguir resume algumas das alterações mais importantes para este tópico.

| Data | Descrição |Motivo|
| --- | ---         | ---   |
| 1º de maio de 2019 | Adicionadas atualizações para o Windows Server 2019 |
| 10 de abril de 2018 | Adicionada uma discussão sobre quando as personalizações do usuário no menu Iniciar são perdidas após uma atualização do sistema operacional in-loco|Problema de texto explicativo conhecido. |
| 13 de março de 2018 | Atualizado para o Windows Server 2016 | Movido para fora da biblioteca de versões anteriores e atualizado para a versão atual do Windows Server. |
| 13 de abril de 2017 | Adicionadas informações de perfil para o Windows 10, versão 1703, e esclarecido como as versões do perfil móvel funcionam ao atualizar sistemas operacionais, confira [Considerações ao usar os Perfis de Usuários Móveis em diversas versões do Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows). | Comentários de clientes. |
| 14 de março de 2017 | Adicionada uma etapa opcional para especificar um layout do menu Iniciar obrigatório para PCs com Windows 10 no [Apêndice A: Lista de verificação para a implantação de Perfis de Usuários Móveis](#appendix-a-checklist-for-deploying-roaming-user-profiles). |Alterações de recurso na atualização mais recente do Windows. |
| 23 de janeiro de 2017 | Adicionada uma etapa à [Etapa 4: Opcionalmente, criar um GPO para Perfis de Usuários Móveis](#step-4-optionally-create-a-gpo-for-roaming-user-profiles) para delegar permissões de leitura a usuários autenticados, o que agora é necessário devido a uma atualização de segurança da Política de Grupo.|Alterações de segurança no processamento de Política de Grupo. |
| 29 de dezembro de 2016 | Adicionado um link na [Etapa 8: Habilitar os GPO de Perfis de Usuários Móveis](#step-8-enable-the-roaming-user-profiles-gpo) para facilitar a obtenção de informações sobre como definir a Política de Grupo para computadores primários. Também corrigimos algumas referências às etapas 5 e 6 que tinham os números errados.|Comentários de clientes. |
| 5 de dezembro de 2016 | Adicionadas informações explicando um problema de transferência das configurações do menu iniciar. | Comentários de clientes. |
| 6 de julho de 2016 | Adicionados sufixos de versão de perfil do Windows 10 no [Apêndice B: Informações de referência de versão de perfil](#appendix-b-profile-version-reference-information). O Windows XP e o Windows Server 2003 também foram removidos da lista de sistemas operacionais com suporte. | Atualizações para as novas versões do Windows e removidas informações sobre as versões do Windows que não têm mais suporte. |
| 7 de julho de 2015 | Adicionado o requisito e a etapa para desabilitar a disponibilidade contínua ao usar um servidor de arquivos em cluster. | Compartilhamentos de arquivos em cluster têm desempenho melhor para pequenas gravações (que são típicas com perfis de usuário móvel) quando a disponibilidade contínua está desabilitada. |
| 19 de março de 2014 | Sufixos de versão de perfil em letras maiúsculas (.V2, .V3, .V4) no [Apêndice B: Informações de referência de versão de perfil](#appendix-b-profile-version-reference-information). | Embora o Windows não faça diferenciação entre maiúsculas e minúsculas, se você usa NFS com o compartilhamento de arquivo, é importante ter a capitalização correta (letras maiúsculas) para o sufixo do perfil. |
| 9 de outubro de 2013 | Revisado para Windows Server 2012 R2 e Windows 8.1, alguns pontos esclarecidos e adicionadas as seções [Considerações ao usar os Perfis de Usuários Móveis em diversas versões do Windows](#considerations-when-using-roaming-user-profiles-on-multiple-versions-of-windows) e [Apêndice B: Informações de referência da versão de perfil](#appendix-b-profile-version-reference-information). | Atualizações para nova versão, comentários do cliente. |

## <a name="more-information"></a>Mais informações

- [Implantar Redirecionamento de Pastas, Arquivos Offline e Perfis de Usuários Móveis](deploy-folder-redirection.md)
- [Implantar computadores primários para Redirecionamento de Pastas e Perfis de Usuários Móveis](deploy-primary-computers.md)
- [Implementando gerenciamento de estado do usuário](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc784645(v=ws.10)>)
- [Declaração de suporte da Microsoft sobre dados de perfil do usuário replicados](https://blogs.technet.microsoft.com/askds/2010/09/01/microsofts-support-statement-around-replicated-user-profile-data/)
- [Realizar o sideload de aplicativos com o DISM](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh852635(v=win.10)>)
- [Solução de problemas de empacotamento, implantação e consulta de aplicativos baseados em Windows Runtime](https://msdn.microsoft.com/library/windows/desktop/hh973484.aspx)