---
title: Configurando o controle de acesso do usuário e as permissões
description: Saiba como configurar o controle de acesso do usuário e as permissões usando o Active Directory ou o Azure AD (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 20b311e9330880c2b26e2494aabe27bb04891868
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407037"
---
# <a name="configure-user-access-control-and-permissions"></a>Configurar controle de acesso do usuário e permissões

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Se você ainda não fez isso, familiarize-se com as [Opções de controle de acesso do usuário no centro de administração do Windows](../plan/user-access-options.md)

> [!NOTE]
> O acesso baseado em grupo no centro de administração do Windows não tem suporte em ambientes de grupo de trabalho ou em domínios não confiáveis.

## <a name="gateway-access-role-definitions"></a>Definições de função de acesso do gateway

Há duas funções para acesso ao serviço de gateway do centro de administração do Windows:

**Os usuários do gateway** podem se conectar ao serviço de gateway do centro de administração do Windows para gerenciar servidores por meio desse gateway, mas não podem alterar as permissões de acesso nem o mecanismo de autenticação usado para autenticar no gateway.

**Os administradores de gateway** podem configurar quem obtém acesso e como os usuários se autenticam no gateway. Somente administradores de gateway podem exibir e definir as configurações de acesso no centro de administração do Windows. Os administradores locais no computador do gateway são sempre administradores do serviço de gateway do centro de administração do Windows.

> [!NOTE]
> O acesso ao gateway não implica o acesso a servidores gerenciados visíveis pelo Gateway. Para gerenciar um servidor de destino, o usuário que está se conectando deve usar credenciais (por meio de suas credenciais do Windows passadas ou por meio de credenciais fornecidas na sessão do centro de administração do Windows usando a ação **gerenciar como** ) que tem acesso administrativo para esse servidor de destino.

## <a name="active-directory-or-local-machine-groups"></a>Grupos de computadores Active Directory ou locais

Por padrão, Active Directory ou grupos de computadores locais são usados para controlar o acesso do gateway. Se você tiver um domínio Active Directory, poderá gerenciar o acesso de usuário e administrador de gateway de dentro da interface do centro de administração do Windows.

Na guia **usuários** , você pode controlar quem pode acessar o centro de administração do Windows como um usuário de gateway. Por padrão, e se você não especificar um grupo de segurança, qualquer usuário que acessar a URL do gateway terá acesso. Depois de adicionar um ou mais grupos de segurança à lista de usuários, o acesso é restrito aos membros desses grupos.

Se você não usar um domínio Active Directory em seu ambiente, o acesso será controlado pelos grupos locais `Users` e `Administrators` no computador do gateway do centro de administração do Windows.

### <a name="smartcard-authentication"></a>Autenticação de cartão inteligente

Você pode impor a **autenticação de cartão inteligente** especificando um grupo adicional _necessário_ para grupos de segurança baseados em cartão inteligente. Depois de adicionar um grupo de segurança baseado em cartão inteligente, um usuário só poderá acessar o serviço centro de administração do Windows se eles forem membros de qualquer grupo de segurança e um grupo de cartões inteligentes incluídos na lista de usuários.

Na guia **Administradores** , você pode controlar quem pode acessar o centro de administração do Windows como um administrador de gateway. O grupo local de administradores no computador sempre terá acesso de administrador completo e não poderá ser removido da lista. Ao adicionar grupos de segurança, você concede aos membros desses grupos privilégios para alterar as configurações do gateway do centro de administração do Windows. A lista de administradores dá suporte à autenticação de cartão inteligente da mesma maneira que a lista de usuários: com a condição e para um grupo de segurança e um grupo de cartões inteligentes.

## <a name="azure-active-directory"></a>Active Directory do Azure

Se sua organização usa o Azure Active Directory (AD do Azure), você pode optar por adicionar uma camada **adicional** de segurança ao centro de administração do Windows exigindo a autenticação do Azure ad para acessar o gateway. Para acessar o centro de administração do Windows, a **conta do Windows** do usuário também deve ter acesso ao servidor de gateway (mesmo que a autenticação do Azure ad seja usada). Ao usar o Azure AD, você gerenciará permissões de acesso de usuário e administrador do centro de administração do Windows no portal do Azure, em vez de dentro da interface do usuário do centro de administração do Windows.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>Acessando o centro de administração do Windows quando a autenticação do Azure AD está habilitada

Dependendo do navegador usado, alguns usuários que acessam o centro de administração do Windows com a autenticação do Azure AD configurada receberão um prompt adicional **do navegador** onde precisam fornecer suas credenciais de conta do Windows para o computador no qual O centro de administração do Windows está instalado. Depois de inserir essas informações, os usuários receberão o prompt de autenticação Azure Active Directory adicional, que exige as credenciais de uma conta do Azure que tenha recebido acesso no aplicativo Azure AD no Azure.

> [!NOTE]
> Os usuários que são conta do Windows têm **direitos de administrador** no computador do gateway não serão solicitados para a autenticação do Azure AD.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configurando a autenticação Azure Active Directory para a visualização do centro de administração do Windows

Vá para **configurações**do centro de administração do Windows  > **acessar** e use o comutador de alternância para ativar "usar Azure Active Directory para adicionar uma camada de segurança ao gateway". Se você não registrou o gateway no Azure, você será guiado para fazer isso no momento.

Por padrão, todos os membros do locatário do Azure AD têm acesso de usuário ao serviço de gateway do centro de administração do Windows. Somente os administradores locais no computador do gateway têm acesso de administrador ao gateway do centro de administração do Windows. Observe que os direitos dos administradores locais no computador do gateway não podem ser restritos. os administradores locais podem fazer qualquer coisa, independentemente de o Azure AD ser usado para autenticação.

Se você quiser conceder acesso ao serviço do centro de administração do Windows a usuários ou grupos específicos do Azure AD ou administrador de gateway, você deve fazer o seguinte:

1.  Vá para o aplicativo do Azure AD do centro de administração do Windows no portal do Azure usando o hiperlink fornecido nas configurações de acesso. Observe que esse hiperlink só está disponível quando a autenticação Azure Active Directory está habilitada. 
    -   Você também pode encontrar seu aplicativo na portal do Azure acessando **Azure Active Directory** > **aplicativos empresariais** > **todos os aplicativos** e pesquisando **WindowsAdminCenter** (o aplicativo Azure ad será nomeado WindowsAdminCenter-<gateway name>). Se você não obtiver nenhum resultado da pesquisa, certifique-se de que **Mostrar** está definido como **todos os aplicativos**, o **status do aplicativo** está definido como **qualquer** e clique em aplicar e tente a pesquisa. Depois de encontrar o aplicativo, vá para **usuários e grupos**
2.  Na guia Propriedades, defina **atribuição de usuário necessária** como Sim.
    Depois de fazer isso, somente os membros listados na guia **usuários e grupos** poderão acessar o gateway do centro de administração do Windows.
3.  Na guia usuários e grupos, selecione **Adicionar usuário**. Você deve atribuir um usuário de gateway ou uma função de administrador de gateway para cada usuário/grupo adicionado.

Depois de ativar a autenticação do Azure AD, o serviço de gateway é reiniciado e você deve atualizar o navegador. Você pode atualizar o acesso do usuário para o aplicativo de SME Azure AD no portal do Azure a qualquer momento.

Os usuários serão solicitados a entrar usando sua identidade de Azure Active Directory quando tentarem acessar a URL do gateway do centro de administração do Windows. Lembre-se de que os usuários também devem ser membros dos usuários locais no servidor de gateway para acessar o centro de administração do Windows.

Os usuários e administradores podem exibir sua conta conectada no momento e, além disso, sair dessa conta do Azure AD na guia **conta** das configurações do centro de administração do Windows.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configurando a autenticação Azure Active Directory para o centro de administração do Windows

[Para configurar a autenticação do Azure AD, primeiro você deve registrar seu gateway com o Azure](azure-integration.md) (você só precisa fazer isso uma vez para o gateway do centro de administração do Windows). Esta etapa cria um aplicativo do Azure AD do qual você pode gerenciar o acesso de administrador de gateway e usuário de gateway.

Se você quiser conceder acesso ao serviço do centro de administração do Windows a usuários ou grupos específicos do Azure AD ou administrador de gateway, você deve fazer o seguinte:

1.  Vá para seu aplicativo de SME Azure AD no portal do Azure. 
    -   Ao clicar em **alterar controle de acesso** e, em seguida, selecionar **Azure Active Directory** nas configurações de acesso do centro de administração do Windows, você pode usar o hiperlink fornecido na interface do usuário para acessar seu aplicativo do Azure AD no portal do Azure. Esse hiperlink também está disponível nas configurações de acesso depois que você clica em salvar e selecionou o Azure AD como seu provedor de identidade de controle de acesso.
    -   Você também pode encontrar seu aplicativo na portal do Azure acessando **Azure Active Directory** > **aplicativos empresariais** > **todos os aplicativos** e pesquisando o **SME** (o aplicativo Azure ad será nomeado SME-<gateway>). Se você não obtiver nenhum resultado da pesquisa, certifique-se de que **Mostrar** está definido como **todos os aplicativos**, o **status do aplicativo** está definido como **qualquer** e clique em aplicar e tente a pesquisa. Depois de encontrar o aplicativo, vá para **usuários e grupos**
2.  Na guia Propriedades, defina **atribuição de usuário necessária** como Sim.
    Depois de fazer isso, somente os membros listados na guia **usuários e grupos** poderão acessar o gateway do centro de administração do Windows.
3.  Na guia usuários e grupos, selecione **Adicionar usuário**. Você deve atribuir um usuário de gateway ou uma função de administrador de gateway para cada usuário/grupo adicionado.

Depois de salvar o controle de acesso do AD do Azure no painel de **controle alterar acesso** , o serviço de gateway é reiniciado e você deve atualizar o navegador. Você pode atualizar o acesso do usuário para o aplicativo do Azure AD do centro de administração do Windows no portal do Azure a qualquer momento. 

Os usuários serão solicitados a entrar usando sua identidade de Azure Active Directory quando tentarem acessar a URL do gateway do centro de administração do Windows. Lembre-se de que os usuários também devem ser membros dos usuários locais no servidor de gateway para acessar o centro de administração do Windows. 

Usando a guia **Azure** das configurações gerais do centro de administração do Windows, os usuários e os administradores podem exibir sua conta conectada no momento e, além disso, sair dessa conta do Azure AD.

### <a name="conditional-access-and-multi-factor-authentication"></a>Acesso condicional e autenticação multifator

Um dos benefícios de usar o Azure AD como uma camada adicional de segurança para controlar o acesso ao gateway do centro de administração do Windows é que você pode aproveitar os poderosos recursos de segurança do Azure AD, como o acesso condicional e a autenticação multifator. 

[Saiba mais sobre como configurar o acesso condicional com o Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configurar logon único

**Logon único quando implantado como um serviço no Windows Server**

Quando você instala o centro de administração do Windows no Windows 10, ele está pronto para usar o logon único. No entanto, se você pretende usar o centro de administração do Windows no Windows Server, precisará configurar alguma forma de delegação de Kerberos em seu ambiente para poder usar o logon único. A delegação configura o computador do gateway como confiável para delegar ao nó de destino. 

Para configurar a [delegação restrita baseada em recursos](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview) em seu ambiente, execute os seguintes cmdlets do PowerShell. (Lembre-se de que isso requer um controlador de domínio executando o Windows Server 2012 ou posterior).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

Neste exemplo, o gateway do centro de administração do Windows está instalado no servidor **WindowsAdminCenterGW**e o nome do nó de destino é **ManagedNode**.

Para remover essa relação, execute o seguinte cmdlet:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Controle de acesso baseado em função

O controle de acesso baseado em função permite que você forneça acesso limitado ao computador aos usuários em vez de torná-los administradores locais completos.
[Leia mais sobre o controle de acesso baseado em função e as funções disponíveis.](../plan/user-access-options.md#role-based-access-control)

A configuração do RBAC consiste em duas etapas: Habilitando o suporte no (s) computador (es) de destino e atribuindo usuários às funções relevantes.

> [!TIP]
> Verifique se você tem privilégios de administrador local nos computadores em que você está configurando o suporte para o controle de acesso baseado em função.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Aplicar o controle de acesso baseado em função a um único computador

O modelo de implantação de máquina única é ideal para ambientes simples com apenas alguns computadores a serem gerenciados.
Configurar um computador com suporte para o controle de acesso baseado em função resultará nas seguintes alterações:

-   Os módulos do PowerShell com funções exigidas pelo centro de administração do Windows serão instalados na unidade do sistema, em `C:\Program Files\WindowsPowerShell\Modules`. Todos os módulos serão iniciados com **o Microsoft. SME**
-   A configuração de estado desejado executará uma configuração única para configurar um ponto de extremidade de administração apenas suficiente no computador, chamado **Microsoft. SME. PowerShell**. Esse ponto de extremidade define as 3 funções usadas pelo centro de administração do Windows e será executado como um administrador local temporário quando um usuário se conectar a ele.
-   3 novos grupos locais serão criados para controlar quais usuários recebem acesso a quais funções:
    -   Administradores do centro de administração do Windows
    -   Administradores do Hyper-V do centro de administração do Windows
    -   Leitores do centro de administração do Windows

Para habilitar o suporte para o controle de acesso baseado em função em um único computador, siga estas etapas:

1.  Abra o centro de administração do Windows e conecte-se ao computador que você deseja configurar com o controle de acesso baseado em função usando uma conta com privilégios de administrador local no computador de destino.
2.  Na ferramenta **visão geral** , clique em **configurações** > **controle de acesso baseado em função**.
3.  Clique em **aplicar** na parte inferior da página para habilitar o suporte ao controle de acesso baseado em função no computador de destino. O processo do aplicativo envolve a cópia de scripts do PowerShell e a invocação de uma configuração (usando a configuração de estado desejado do PowerShell) no computador de destino. Pode levar até 10 minutos para ser concluído e resultará na reinicialização do WinRM. Isso desconectará temporariamente os usuários do centro de administração do Windows, do PowerShell e do WMI.
4.  Atualize a página para verificar o status do controle de acesso baseado em função. Quando estiver pronto para uso, o status será alterado para **aplicado**.

Depois que a configuração for aplicada, você poderá atribuir usuários às funções:

1.  Abra a ferramenta **usuários e grupos locais** e navegue até a guia **grupos** .
2.  Selecione o grupo de **leitores do centro de administração do Windows** .
3.  No painel de *detalhes* na parte inferior, clique em **Adicionar usuário** e insira o nome de um usuário ou grupo de segurança que deve ter acesso somente leitura ao servidor por meio do centro de administração do Windows. Os usuários e grupos podem vir do computador local ou do seu Active Directory domínio.
4.  Repita as etapas 2-3 para os grupos de administradores do **Hyper-V do Windows Admin Center** e do **centro de administração do Windows** .

Você também pode preencher esses grupos de forma consistente em seu domínio, configurando um objeto de Política de Grupo com a [configuração de política de grupos restritos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Aplicar o controle de acesso baseado em função a vários computadores

Em uma grande implantação corporativa, você pode usar suas ferramentas de automação existentes para enviar por push o recurso de controle de acesso baseado em função para seus computadores baixando o pacote de configuração do gateway do centro de administração do Windows.
O pacote de configuração foi projetado para ser usado com a configuração de estado desejado do PowerShell, mas você pode adaptá-lo para trabalhar com sua solução de automação preferida.

#### <a name="download-the-role-based-access-control-configuration"></a>Baixar a configuração do controle de acesso baseado em função

Para baixar o pacote de configuração de controle de acesso baseado em função, você precisará ter acesso ao centro de administração do Windows e a um prompt do PowerShell.

Se você estiver executando o gateway do centro de administração do Windows no modo de serviço no Windows Server, use o comando a seguir para baixar o pacote de configuração.
Certifique-se de atualizar o endereço do gateway com o correto para o seu ambiente.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Se você estiver executando o gateway do centro de administração do Windows em seu computador com Windows 10, execute o seguinte comando em vez disso:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Ao expandir o arquivo zip, você verá a seguinte estrutura de pastas:

- InstallJeaFeatures. ps1
- JustEnoughAdministration (diretório)
- Módulos (diretório)
    - Microsoft. SME. \* (diretórios)
    - WindowsAdminCenter. Jea (diretório)

Para configurar o suporte para o controle de acesso baseado em função em um nó, você precisa executar as seguintes ações:

1.  Copie os módulos JustEnoughAdministration, Microsoft. SME. \* e WindowsAdminCenter. Jea para o diretório de módulo do PowerShell no computador de destino. Normalmente, isso está localizado em `C:\Program Files\WindowsPowerShell\Modules`.
2.  Atualize o arquivo **InstallJeaFeature. ps1** para corresponder à configuração desejada para o ponto de extremidade do RBAC.
3.  Execute InstallJeaFeature. ps1 para compilar o recurso de DSC.
4.  Implante sua configuração DSC em todos os seus computadores para aplicar a configuração.

A seção a seguir explica como fazer isso usando a comunicação remota do PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Implantar em vários computadores

Para implantar a configuração que você baixou em vários computadores, você precisará atualizar o script **InstallJeaFeatures. ps1** para incluir os grupos de segurança apropriados para o seu ambiente, copiar os arquivos para cada um dos seus computadores e invocar o scripts de configuração.
Você pode usar suas ferramentas de automação preferenciais para fazer isso, no entanto, este artigo se concentrará em uma abordagem pura baseada no PowerShell.

Por padrão, o script de configuração criará grupos de segurança locais no computador para controlar o acesso a cada uma das funções.
Isso é adequado para computadores ingressados no domínio e no grupo de trabalho, mas se você estiver implantando em um ambiente somente de domínio, talvez queira associar diretamente um grupo de segurança de domínio a cada função.
Para atualizar a configuração para usar grupos de segurança de domínio, abra **InstallJeaFeatures. ps1** e faça as seguintes alterações:

1.  Remova os 3 recursos do **grupo** do arquivo:
    1.  "Grupo MS-Readers-Group"
    2.  "Grupo MS-Hyper-V-Administrators-Group"
    3.  "Grupo MS-Administrators-Group"
2.  Remover os 3 recursos de grupo da propriedade **dependy** de JeaEndpoint
    1.  "[Grupo] MS-Readers-Group"
    2.  "[Grupo] MS-Hyper-V-Administrators-Group"
    3.  "[Grupo] MS-Administrators-Group"
3.  Altere os nomes de grupo na propriedade JeaEndpoint **RoleDefinitions** para os grupos de segurança desejados. Por exemplo, se você tiver um grupo de segurança *CONTOSO\MyTrustedAdmins* que deve receber acesso à função Administradores do centro de administração do Windows, altere `'$env:COMPUTERNAME\Windows Admin Center Administrators'` para `'CONTOSO\MyTrustedAdmins'`. As três cadeias de caracteres que você precisa atualizar são:
    1.  ' $env: administradores do centro de administração do COMPUTERNAME\Windows '
    2.  ' $env: COMPUTERNAME\Windows Admin Center Hyper-V Administrators '
    3.  ' $env: leitores do centro de administração do COMPUTERNAME\Windows '

> [!NOTE]
> Certifique-se de usar grupos de segurança exclusivos para cada função. A configuração falhará se o mesmo grupo de segurança for atribuído a várias funções.

Em seguida, no final do arquivo **InstallJeaFeatures. ps1** , adicione as seguintes linhas do PowerShell à parte inferior do script:

```powershell
Copy-Item "$PSScriptRoot\JustEnoughAdministration" "$env:ProgramFiles\WindowsPowerShell\Modules" -Recurse -Force
$ConfigData = @{
    AllNodes = @()
    ModuleBasePath = @{
        Source = "$PSScriptRoot\Modules"
        Destination = "$env:ProgramFiles\WindowsPowerShell\Modules"
    }
}
InstallJeaFeature -ConfigurationData $ConfigData | Out-Null
Start-DscConfiguration -Path "$PSScriptRoot\InstallJeaFeature" -JobName "Installing JEA for Windows Admin Center" -Force
```

Por fim, você pode copiar a pasta que contém os módulos, o recurso DSC e a configuração para cada nó de destino e executar o script **InstallJeaFeature. ps1** .
Para fazer isso remotamente a partir de sua estação de trabalho de administração, você pode executar os seguintes comandos:

```powershell
$ComputersToConfigure = 'MyServer01', 'MyServer02'

$ComputersToConfigure | ForEach-Object {
    $session = New-PSSession -ComputerName $_ -ErrorAction Stop
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC\JustEnoughAdministration\" -Destination "$env:ProgramFiles\WindowsPowerShell\Modules\" -ToSession $session -Recurse -Force
    Copy-Item -Path "~\Desktop\WindowsAdminCenter_RBAC" -Destination "$env:TEMP\WindowsAdminCenter_RBAC" -ToSession $session -Recurse -Force
    Invoke-Command -Session $session -ScriptBlock { Import-Module JustEnoughAdministration; & "$env:TEMP\WindowsAdminCenter_RBAC\InstallJeaFeature.ps1" } -AsJob
    Disconnect-PSSession $session
}
```
