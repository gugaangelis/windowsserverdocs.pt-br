---
title: Configuração de permissões e controle de acesso do usuário
description: Saiba como configurar o controle de acesso de usuário e permissões usando o Active Directory ou Azure AD (projeto Paulo)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/19/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b19657f4ce1a1a2cfb94f7234f07805ba0abd42c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850567"
---
# <a name="configure-user-access-control-and-permissions"></a>Configurar as permissões e controle de acesso do usuário

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Se você ainda não fez isso, você se familiarizar com o [opções de controle de acesso do usuário no Windows Admin Center](../plan/user-access-options.md)

>[!NOTE]
> Não há suporte para acesso de grupo com base no Windows Admin Center em ambientes de grupo de trabalho ou em domínios não confiáveis.

## <a name="gateway-access-role-definitions"></a>Definições de função de acesso do gateway

Há duas funções para acessar o serviço de gateway do Windows Admin Center:

**Usuários do gateway** pode se conectar ao serviço de gateway do Windows Admin Center para gerenciar servidores por meio do gateway, mas eles não podem alterar as permissões de acesso nem o mecanismo de autenticação usado para autenticar para o gateway.

**Os administradores de gateway** pode configurar a quem obtém acesso, bem como os usuários são autenticados para o gateway. Somente os administradores de gateway podem exibir e definir as configurações de acesso no Windows Admin Center. Os administradores locais no computador do gateway são sempre os administradores do serviço de gateway do Windows Admin Center.

> [!NOTE]
> Acesso ao gateway não implica em acesso aos servidores gerenciados visível pelo gateway. Para gerenciar um servidor de destino, o usuário conectado deve usar credenciais (por meio de suas credenciais do Windows passadas ou por meio de credenciais fornecidas no Windows Admin Center sessão usando o **gerenciar como** ação) que têm acesso administrativo para esse servidor de destino.

## <a name="active-directory-or-local-machine-groups"></a>Active Directory ou grupos de computadores locais

Por padrão, o Active Directory ou grupos de computadores locais são usados para controlar o acesso do gateway. Se você tiver um domínio do Active Directory, você pode gerenciar o gateway de usuário e administrador de acesso de dentro da interface do Windows Admin Center.

Sobre o **usuários** guia, você pode controlar quem tem acesso à Windows Admin Center como um usuário do gateway. Por padrão, e se você não especificar um grupo de segurança, qualquer usuário que acessa a URL do gateway tem acesso. Depois de adicionar um ou mais grupos de segurança à lista de usuários, o acesso é restrito aos membros desses grupos.

Se você não usar um domínio do Active Directory em seu ambiente, o acesso é controlado pelo ```Users``` e ```Administrators``` grupos locais no computador do gateway do Windows Admin Center.

### <a name="smartcard-authentication"></a>Autenticação de cartão inteligente

Você pode impor **autenticação de cartão inteligente** especificando adicional _necessária_ grupo para grupos de segurança baseada em cartão inteligente. Depois de adicionar um grupo de segurança baseada em cartão inteligente, um usuário pode acessar somente o serviço Windows Admin Center se eles são membros de qualquer grupo de segurança e um grupo de cartão inteligente é incluído na lista de usuários.

Sobre o **administradores** guia, você pode controlar quem tem acesso à Windows Admin Center como um administrador de gateway. O grupo de administradores locais no computador sempre terá acesso de administrador completo e não pode ser removido da lista. Adicionando grupos de segurança, você permitir que membros esses privilégios de grupos para alterar as configurações de gateway do Windows Admin Center. A lista de administradores oferece suporte à autenticação de cartão inteligente da mesma maneira como a lista de usuários: a condição AND para um grupo de segurança e um grupo de cartão inteligente.

## <a name="azure-active-directory"></a>Azure Active Directory

Se sua organização usa o Azure Active Directory (Azure AD), você pode optar por adicionar um **adicionais** camada de segurança para Windows Admin Center, exigindo a autenticação do Azure AD para acessar o gateway. Para acessar a Windows Admin Center, o usuário **conta do Windows** também deve ter acesso ao servidor de gateway (mesmo se a autenticação do AD do Azure é usada). Quando você usa o Azure AD, você gerenciará Windows Admin Center permissões de acesso de usuário e administrador do Portal do Azure, em vez de dentro a interface do usuário do Windows Admin Center.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>Acessando o Windows Admin Center quando a autenticação do Azure AD está habilitada

Dependendo do navegador usado, alguns usuários que acessam a Windows Admin Center com a autenticação do Azure AD configurada receberá um prompt adicional **do navegador** onde elas precisam fornecer suas credenciais de conta do Windows para o computador no qual o Windows Admin Center é instalado. Depois de inserir essas informações, os usuários receberá o prompt de autenticação adicional do Active Directory do Azure, que exige as credenciais de uma conta do Azure que concedeu acesso no aplicativo do Azure AD no Azure.

> [!NOTE]
> Os usuários do Windows conta tem **direitos de administrador** no gateway do computador não será solicitado para a autenticação do AD do Azure.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Configurando a autenticação do Active Directory do Azure para Windows Admin Center Preview

Vá para o Windows Admin Center **configurações** > **acesso** e use a opção de ativar/desativar para ativar o "usar Azure Active Directory para adicionar uma camada de segurança para o gateway". Se você não tiver registrado o gateway para o Azure, você será guiado para fazer isso no momento.

Por padrão, todos os membros do locatário do Azure AD têm acesso de usuário para o serviço de gateway do Windows Admin Center. Somente os administradores locais no computador do gateway tem acesso de administrador para o gateway do Windows Admin Center. Observe que os direitos de administradores locais no computador do gateway não podem ser restritos - os administradores locais podem fazer qualquer coisa, independentemente se o AD do Azure é usado para autenticação.

Se você quiser forneça específico do Azure AD aos usuários ou usuário do gateway de grupos ou acesso de administrador do gateway para o serviço Windows Admin Center, faça o seguinte:

1.  Vá para seu aplicativo Windows Admin Center do Azure AD no portal do Azure usando o hiperlink fornecido nas configurações de acesso. Observe que este hiperlink está disponível apenas quando a autenticação do Active Directory do Azure está habilitada. 
    -   Você também pode encontrar seu aplicativo no portal do Azure, vá para **Azure Active Directory** > **aplicativos empresariais** > **detodososaplicativos** e pesquisando **WindowsAdminCenter** (o aplicativo do Azure AD será denominado WindowsAdminCenter -<gateway name>). Se você não obtiver os resultados da pesquisa, certifique-se **mostram** é definido como **todos os aplicativos**, **status do aplicativo** é definido como **qualquer** e clique em Aplicar, em seguida, tente sua pesquisa. Depois de localizar o aplicativo, vá para **usuários e grupos**
2.  Na guia Propriedades, defina **atribuição de usuário necessária** como Sim.
    Depois de ter feito isso, somente os membros listados na **usuários e grupos** guia poderão acessar o gateway do Windows Admin Center.
3.  Na guia usuários e grupos, selecione **adicionar usuário**. Você deve atribuir um usuário de gateway ou a função de administrador do gateway para cada usuário/grupo adicionado.

Depois de ativar a autenticação do Azure AD, a reinicialização do serviço de gateway e você deve atualizar seu navegador. Você pode atualizar o acesso do usuário para o aplicativo de SME do Azure AD no portal do Azure a qualquer momento.

Os usuários serão solicitados a entrar usando suas identidades do Active Directory do Azure quando eles tentam acessar a URL de gateway do Windows Admin Center. Lembre-se de que os usuários também devem ser um membro dos usuários locais no servidor gateway para acessar a Windows Admin Center.

Os usuários e administradores podem exibir sua conta está registrado no momento e também como saída desta conta do Azure AD do **conta** guia de configurações do Windows Admin Center.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Configurando a autenticação do Active Directory do Azure para Windows Admin Center

[Para configurar a autenticação do Azure AD, você deve primeiro registrar seu gateway com o Azure](azure-integration.md) (você só precisa fazer isso vez para o gateway do Windows Admin Center). Esta etapa cria um aplicativo do Azure AD do qual você pode gerenciar o acesso de administrador do gateway e de usuário do gateway.

Se você quiser forneça específico do Azure AD aos usuários ou usuário do gateway de grupos ou acesso de administrador do gateway para o serviço Windows Admin Center, faça o seguinte:

1.  Vá para seu aplicativo de SME do Azure AD no portal do Azure. 
    -   Quando você clica em **controle de acesso de alteração** e, em seguida, selecione **Azure Active Directory** das configurações de acesso do Windows Admin Center, você pode usar o hiperlink fornecido na interface do usuário para acessar o Azure AD aplicativo no portal do Azure. Este hiperlink também está disponível nas configurações de acesso depois que você clique em Salvar e tiver selecionado o Azure AD como seu provedor de identidade de controle de acesso.
    -   Você também pode encontrar seu aplicativo no portal do Azure, vá para **Azure Active Directory** > **aplicativos empresariais** > **detodososaplicativos** e pesquisando **SME** (o aplicativo do Azure AD será denominado SME -<gateway>). Se você não obtiver os resultados da pesquisa, certifique-se **mostram** é definido como **todos os aplicativos**, **status do aplicativo** é definido como **qualquer** e clique em Aplicar, em seguida, tente sua pesquisa. Depois de localizar o aplicativo, vá para **usuários e grupos**
2.  Na guia Propriedades, defina **atribuição de usuário necessária** como Sim.
    Depois de ter feito isso, somente os membros listados na **usuários e grupos** guia poderão acessar o gateway do Windows Admin Center.
3.  Na guia usuários e grupos, selecione **adicionar usuário**. Você deve atribuir um usuário de gateway ou a função de administrador do gateway para cada usuário/grupo adicionado.

Depois de salvar o Azure AD controle de acesso na **controle de acesso de alteração** reinicia do painel, o serviço de gateway e você deve atualizar seu navegador. Você pode atualizar o acesso do usuário para o aplicativo do Windows Admin Center do Azure AD no portal do Azure a qualquer momento. 

Os usuários serão solicitados a entrar usando suas identidades do Active Directory do Azure quando eles tentam acessar a URL de gateway do Windows Admin Center. Lembre-se de que os usuários também devem ser um membro dos usuários locais no servidor gateway para acessar a Windows Admin Center. 

Usando o **Azure** guia de configurações gerais do Windows Admin Center, os usuários e administradores pode exibir sua conta está registrado no momento e também como saída desta conta do Azure AD.

### <a name="conditional-access-and-multi-factor-authentication"></a>Acesso condicional e autenticação multifator

Um dos benefícios de usar o Azure AD como uma camada adicional de segurança para controlar o acesso ao gateway do Windows Admin Center é que você pode aproveitar os recursos de segurança avançados do Azure AD como acesso condicional e autenticação multifator. 

[Saiba mais sobre como configurar o acesso condicional ao Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configurar logon único

**O logon único quando implantado como um serviço no Windows Server**

Quando você instala o Windows Admin Center no Windows 10, está pronto para usar o logon único. No entanto, se você pretende usar o Windows Admin Center no Windows Server, você precisará configurar alguma forma de delegação de Kerberos em seu ambiente antes de poder usar o logon único. A delegação configura o computador do gateway como confiáveis para delegar ao nó de destino. 

Para configurar [delegação restrita baseada em recursos](http://windowsitpro.com/security/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1) em seu ambiente, execute os seguintes cmdlets do PowerShell. (Estar ciente de que isso exige um controlador de domínio executando o Windows Server 2012 ou posterior).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

Neste exemplo, o gateway do Windows Admin Center está instalado no servidor **WindowsAdminCenterGW**, e é o nome do nó de destino **ManagedNode**.

Para remover essa relação, execute o seguinte cmdlet:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Controle de acesso baseado em função

Controle de acesso baseado em função permite que você forneça aos usuários com acesso limitado à máquina em vez de administradores locais completo-los de fazer.
[Leia mais sobre o controle de acesso baseado em função e as funções disponíveis.](../plan/user-access-options.md#role-based-access-control)

Como configurar o RBAC consiste nas 2 etapas: habilitar o suporte nos computadores de destino e atribuir usuários às funções relevantes.

> [!TIP]
> Verifique se que você tem privilégios de administrador local nos computadores onde você está configurando o suporte para controle de acesso baseado em função.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Aplicar o controle de acesso baseado em função a uma única máquina

O modelo de implantação de máquina única é ideal para ambientes simples com apenas alguns computadores para gerenciar.
Configurar uma máquina com suporte para controle de acesso baseado em função resultará nas seguintes alterações:
-   Módulos do PowerShell com as funções exigidas pelo Windows Admin Center serão instalados na unidade do sistema, em `C:\Program Files\WindowsPowerShell\Modules`. Todos os módulos começarão com **Microsoft.Sme**
-   Desired State Configuration será executado em uma única configuração para configurar um ponto de extremidade de administração Just Enough no computador, nomeado **Microsoft.Sme.PowerShell**. Esse ponto de extremidade define as 3 funções usadas pelo Windows Admin Center e será executado como um administrador local temporário, quando um usuário se conecta a ele.
-   3 novos grupos locais serão criados para controlar quais usuários recebem acesso a quais funções:
    -   Administradores do Windows Admin Center
    -   Administradores do Hyper-V do Windows Admin Center
    -   Leitores de Windows Admin Center

Para habilitar o suporte para controle de acesso baseado em função em um único computador, siga estas etapas:

1.  Abra o Windows Admin Center e conectar-se ao computador que você deseja configurar com o controle de acesso baseado em função usando uma conta com privilégios de administrador local no computador de destino.
2.  Sobre o **visão geral** ferramenta, clique em **configurações** > **controle de acesso baseado em função**.
3.  Clique em **aplicar** na parte inferior da página para habilitar o suporte para controle de acesso baseado em função no computador de destino. O processo do aplicativo envolve copiar scripts do PowerShell e invocar uma configuração (usando o PowerShell Desired State Configuration) no computador de destino. Ele pode levar até 10 minutos para ser concluída e resultará no reinício do WinRM. Isso desconectará temporariamente os usuários de Windows Admin Center, o PowerShell e o WMI.
4.  Atualize a página para verificar o status de controle de acesso baseado em função. Quando estiver pronto para uso, o status será alterado para **aplicada**.

Depois que a configuração é aplicada, você pode atribuir usuários às funções:

1.  Abra o **usuários e grupos locais** de ferramentas e navegue até a **grupos** guia.
2.  Selecione o **Windows Admin Center leitores** grupo.
3.  No *detalhes* painel na parte inferior, clique em **adicionar usuário** e insira o nome de um usuário ou grupo de segurança que deve ter acesso somente leitura para o servidor por meio do Windows Admin Center. Os usuários e grupos podem vir de computador local ou domínio do Active Directory.
4.  Repita as etapas 2 e 3 para o **os administradores do Hyper-V do Windows Admin Center** e **os administradores do Windows Admin Center** grupos.

Você também pode preencher esses grupos consistentemente em seu domínio por meio da configuração de um objeto de diretiva de grupo com o [configuração de diretiva de grupos restritos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Aplicar o controle de acesso baseado em função para vários computadores

Em uma implantação de grande porte, você pode usar as ferramentas de automação existentes para enviar por push o recurso de controle de acesso baseado em função para seus computadores baixando o pacote de configuração do gateway do Windows Admin Center.
O pacote de configuração foi projetado para ser usado com o PowerShell Desired State Configuration, mas você pode adaptá-lo para trabalhar com sua solução de automação preferencial.

#### <a name="download-the-role-based-access-control-configuration"></a>Baixar a configuração de controle de acesso baseado em função

Para baixar o pacote de configuração de controle de acesso baseado em função, você precisará ter acesso ao Windows Admin Center e um prompt do PowerShell.

Se você estiver executando o gateway do Windows Admin Center no modo de serviço no Windows Server, use o comando a seguir para baixar o pacote de configuração.
Certifique-se de atualizar o endereço do gateway com uma correta para seu ambiente.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Se você estiver executando o gateway do Windows Admin Center em seu computador Windows 10, execute o seguinte comando:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Quando você expande o arquivo zip, você verá a seguinte estrutura de pasta:
- InstallJeaFeatures.ps1
- JustEnoughAdministration (diretório)
- Módulos (diretório)
    - Microsoft.SME. \* (diretórios)
    - WindowsAdminCenter.Jea (diretório)

Para configurar o suporte para controle de acesso baseado em função em um nó, você precisará executar as seguintes ações:
1.  Copie o JustEnoughAdministration, Microsoft.SME. \*e os módulos de WindowsAdminCenter.Jea para o diretório do módulo do PowerShell no computador de destino. Normalmente, isso está localizado em `C:\Program Files\WindowsPowerShell\Modules`.
2.  Atualização **InstallJeaFeature.ps1** arquivo para corresponder à configuração desejada para o ponto de extremidade do RBAC.
3.  Execute InstallJeaFeature.ps1 para compilar o recurso de DSC.
4.  Implante sua configuração de DSC em todas as suas máquinas para aplicar a configuração.

A seção a seguir explica como fazer isso usando a comunicação remota do PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Implantar em vários computadores

Para implantar a configuração é baixada em vários computadores, você precisará atualizar o **InstallJeaFeatures.ps1** script para incluir grupos de segurança apropriados para seu ambiente, copie os arquivos para cada um dos seus computadores, e invocar os scripts de configuração.
Você pode usar suas ferramentas de automação preferencial para fazer isso, no entanto, este artigo se concentra em uma abordagem essencialmente baseada no PowerShell.

Por padrão, o script de configuração criará grupos de segurança local no computador para controlar o acesso a cada uma das funções.
Isso é adequado para o grupo de trabalho e domínio computadores associados ao, mas se você estiver implantando em um ambiente de domínio somente, talvez você queira diretamente associar um grupo de segurança de domínio com cada função.
Para atualizar a configuração para usar grupos de segurança de domínio, abra **InstallJeaFeatures.ps1** e faça as seguintes alterações:

1.  Remover o 3 **grupo** recursos do arquivo:
    1.  "Grupo de leitores MS do grupo"
    2.  "Grupo MS-Hyper-V-administradores-Group"
    3.  "Grupo de administradores do grupo MS"
2.  Remova os recursos do grupo 3 o JeaEndpoint **DependsOn** propriedade
    1.  "[Group]MS-Readers-Group"
    2.  "[Group]MS-Hyper-V-Administrators-Group"
    3.  "[Group]MS-Administrators-Group"
3.  Altere os nomes de grupo no JeaEndpoint **RoleDefinitions** propriedade aos grupos de segurança desejado. Por exemplo, se você tiver um grupo de segurança *CONTOSO\MyTrustedAdmins* que devem receber acesso à função administradores do Windows Admin Center, alteração `'$env:COMPUTERNAME\Windows Admin Center Administrators'` para `'CONTOSO\MyTrustedAdmins'`. São as três cadeias de caracteres que você precisa atualizar:
    1.  ' $env: COMPUTERNAME\Windows Admin Center administradores
    2.  ' $env: Administradores do Hyper-V COMPUTERNAME\Windows Admin Center
    3.  ' $env: COMPUTERNAME\Windows Admin Center Readers '

> [!NOTE]
> Certifique-se de usar grupos de segurança exclusivo para cada função. Configuração falhará se o mesmo grupo de segurança for atribuído a várias funções.

Em seguida, no final o **InstallJeaFeatures.ps1** arquivo, adicione as seguintes linhas do PowerShell para a parte inferior do script:

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

Por fim, você pode copiar a pasta que contém os módulos, recursos de DSC e configuração para cada nó de destino e executar o **InstallJeaFeature.ps1** script.
Para fazer isso remotamente em sua estação de trabalho do administrador, você pode executar os comandos a seguir:

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
