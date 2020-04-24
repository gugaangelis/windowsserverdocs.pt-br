---
title: Como configurar o controle de acesso do usuário e as permissões
description: Saiba como configurar o controle de acesso do usuário e as permissões usando o Active Directory ou o Azure AD (Projeto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/07/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 39af45506ff7023cebe437992e90f6d4ec051333
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "79323588"
---
# <a name="configure-user-access-control-and-permissions"></a>Configurar o controle de acesso do usuário e as permissões

> Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

Se você ainda não fez isso, familiarize-se com as [opções de controle de acesso do usuário no Windows Admin Center](../plan/user-access-options.md)

> [!NOTE]
> O acesso baseado em grupo no Windows Admin Center não tem suporte em ambientes de grupo de trabalho ou em domínios não confiáveis.

## <a name="gateway-access-role-definitions"></a>Definições de função de acesso do gateway

Há duas funções para acesso ao serviço de gateway do Windows Admin Center:

Os **usuários de gateway** podem se conectar ao serviço de gateway do Windows Admin Center para gerenciar servidores por meio desse gateway, mas não podem alterar permissões de acesso nem o mecanismo de autenticação usado para autenticação no gateway.

Os **administradores de gateway** podem configurar quem obtém acesso e como os usuários se autenticam no gateway. Somente administradores de gateway podem exibir e definir as configurações de Acesso no Windows Admin Center. Os administradores locais no computador do gateway são sempre administradores do serviço de gateway do Windows Admin Center.

> [!NOTE]
> O acesso ao gateway não implica o acesso a servidores gerenciados visíveis pelo gateway. Para gerenciar um servidor de destino, o usuário que está se conectando deve usar credenciais (por meio de suas credenciais do Windows passadas ou por meio de credenciais fornecidas na sessão do Windows Admin Center usando a ação **Gerenciar como**) que têm acesso administrativo a esse servidor de destino.

## <a name="active-directory-or-local-machine-groups"></a>Grupos de computadores locais ou Active Directory

Por padrão, o Active Directory ou grupos de computadores locais são usados para controlar o acesso do gateway. Se você tiver um domínio do Active Directory, poderá gerenciar o acesso de usuário e administrador de gateway de dentro da interface do Windows Admin Center.

Na guia **Usuários**, você pode controlar quem pode acessar o Windows Admin Center como um usuário de gateway. Por padrão, se você não especificar um grupo de segurança, qualquer usuário que acessar a URL do gateway terá acesso. Depois de adicionar um ou mais grupos de segurança à lista de usuários, o acesso será restrito aos membros desses grupos.

Se você não usar um domínio Active Directory em seu ambiente, o acesso será controlado pelos grupos locais `Users` e `Administrators` no computador do gateway do Windows Admin Center.

### <a name="smartcard-authentication"></a>Autenticação de cartão inteligente

Você pode impor a **autenticação de cartão inteligente** especificando um grupo _obrigatório_ adicional para grupos de segurança baseados em cartão inteligente. Depois de adicionar um grupo de segurança baseado em cartão inteligente, um usuário só poderá acessar o serviço do Windows Admin Center se for membro de qualquer grupo de segurança E houver um grupo de cartões inteligentes incluídos na lista de usuários.

Na guia **Administradores**, você pode controlar quem pode acessar o Windows Admin Center como um administrador de gateway. O grupo local de administradores no computador sempre terá acesso de administrador completo e não poderá ser removido da lista. Ao adicionar grupos de segurança, você concede aos membros desses grupos privilégios para alterar as configurações do gateway do Windows Admin Center. A lista de administradores dá suporte à autenticação de cartão inteligente da mesma maneira que a lista de usuários: com a condição E para um grupo de segurança e um grupo de cartões inteligentes.

## <a name="azure-active-directory"></a>Active Directory do Azure

Se sua organização usa o Azure AD (Azure Active Directory), você pode optar por adicionar uma camada de segurança **adicional** ao Windows Admin Center exigindo a autenticação do Azure AD para acessar o gateway. Para acessar o Windows Admin Center, a **conta do Windows** do usuário também deve ter acesso ao servidor de gateway (mesmo que a autenticação do Azure AD seja usada). Ao usar o Azure AD, você gerenciará permissões de acesso de usuário e administrador do Windows Admin Center no portal do Azure, em vez de gerenciá-las de dentro da interface do usuário do Windows Admin Center.

### <a name="accessing-windows-admin-center-when-azure-ad-authentication-is-enabled"></a>Como acessar o Windows Admin Center quando a autenticação do Azure AD está habilitada

Dependendo do navegador usado, alguns usuários que acessam o Windows Admin Center com a autenticação do Azure AD configurada receberão um aviso adicional **do navegador** em que eles precisam fornecer suas credenciais de conta do Windows para o computador no qual o Windows Admin Center está instalado. Depois de inserir essas informações, os usuários receberão o prompt de autenticação adicional do Azure Active Directory, que exige as credenciais de uma conta do Azure que tenha recebido acesso no aplicativo Azure AD no Azure.

> [!NOTE]
> Os usuários cuja conta do Windows têm **direitos de administrador** no computador do gateway não serão solicitados a informarem a autenticação do Azure AD.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center-preview"></a>Como configurar a autenticação do Azure Active Directory para a Versão Prévia do Windows Admin Center

Acesse **Configurações** > **Acesso** do Windows Admin Center e use a opção de alternância para ativar a opção "Usar Azure Active Directory para adicionar uma camada de segurança ao gateway". Se você não tiver registrado o gateway no Azure, será guiado para fazer isso no momento.

Por padrão, todos os membros do locatário do Azure AD têm acesso de usuário ao serviço de gateway do Windows Admin Center. Somente os administradores locais no computador do gateway têm acesso de administrador ao gateway do Windows Admin Center. Observe que os direitos dos administradores locais no computador do gateway não podem ser restritos. Os administradores locais podem fazer qualquer coisa, independentemente de o Azure AD ser usado para autenticação.

Se você quiser conceder a usuários ou grupos específicos do Azure AD acesso de administrador de gateway ou usuário de gateway ao serviço do Windows Admin Center, deverá fazer o seguinte:

1.  Acesse o aplicativo Azure AD do Windows Admin Center no portal do Azure usando o hiperlink fornecido em Configurações de Acesso. Observe que esse hiperlink só está disponível quando a autenticação Azure Active Directory está habilitada. 
    -   Você também pode encontrar seu aplicativo no portal do Azure acessando **Azure Active Directory** > **Aplicativos empresariais** > **Todos os aplicativos** e pesquisando **WindowsAdminCenter** (o aplicativo Azure AD terá o nome WindowsAdminCenter-<gateway name>). Se você não obtiver nenhum resultado da pesquisa, verifique se **Mostrar** está definido como **Todos os aplicativos**, **Status do aplicativo** está definido como **Qualquer**, então clique em Aplicar e tente a pesquisa. Depois de encontrar o aplicativo, acesse **Usuários e grupos**
2.  Na guia Propriedades, defina **Atribuição de usuário necessária** como Sim.
    Depois de fazer isso, somente os membros listados na guia **Usuários e grupos** poderão acessar o gateway do Windows Admin Center.
3.  Na guia Usuários e grupos, selecione **Adicionar usuário**. Você deve atribuir uma função de usuário de gateway ou administrador de gateway a cada usuário/grupo adicionado.

Depois de ativar a autenticação do Azure AD, o serviço de gateway é reiniciado e você deve atualizar o navegador. Você pode atualizar o acesso do usuário para o aplicativo do Azure AD do SEM no portal do Azure a qualquer momento.

Os usuários serão solicitados a entrar usando a identidade do Azure Active Directory quando tentarem acessar a URL do gateway do Windows Admin Center. Lembre-se de que os usuários também devem ser membros dos usuários locais no servidor de gateway para acessarem o Windows Admin Center.

Os usuários e administradores podem exibir a conta conectada no momento e, além disso, sair dessa conta do Azure AD na guia **Conta** das Configurações do Windows Admin Center.

### <a name="configuring-azure-active-directory-authentication-for-windows-admin-center"></a>Como configurar a autenticação do Azure Active Directory para o Windows Admin Center

[Para configurar a autenticação do Azure AD, primeiro você deve registrar seu gateway com o Azure](azure-integration.md) (você só precisa fazer isso uma vez para o gateway do Windows Admin Center). Esta etapa cria um aplicativo do Azure AD do qual você pode gerenciar o acesso de administrador do gateway e usuário do gateway.

Se você quiser conceder a usuários ou grupos específicos do Azure AD acesso de administrador de gateway ou usuário de gateway ao serviço do Windows Admin Center, deverá fazer o seguinte:

1.  Vá para seu aplicativo de SME do Azure AD no portal do Azure. 
    -   Ao clicar em **Alterar controle de acesso** e, em seguida, selecionar **Azure Active Directory** nas configurações de Acesso do Windows Admin Center, você pode usar o hiperlink fornecido na interface do usuário para acessar seu aplicativo do Azure AD no portal do Azure. Esse hiperlink também está disponível nas configurações de acesso depois que você clica em Salvar e seleciona o Azure AD como provedor de identidade de controle de acesso.
    -   Você também pode encontrar seu aplicativo no portal do Azure acessando **Azure Active Directory** > **Aplicativos empresariais** > **Todos os aplicativos** e pesquisando **SME** (o aplicativo Azure AD terá o nome SME-<gateway>). Se você não obtiver nenhum resultado da pesquisa, verifique se **Mostrar** está definido como **Todos os aplicativos**, **Status do aplicativo** está definido como **Qualquer**, então clique em Aplicar e tente a pesquisa. Depois de encontrar o aplicativo, acesse **Usuários e grupos**
2.  Na guia Propriedades, defina **Atribuição de usuário necessária** como Sim.
    Depois de fazer isso, somente os membros listados na guia **Usuários e grupos** poderão acessar o gateway do Windows Admin Center.
3.  Na guia Usuários e grupos, selecione **Adicionar usuário**. Você deve atribuir uma função de usuário de gateway ou administrador de gateway a cada usuário/grupo adicionado.

Depois de salvar o serviço de controle de acesso do Azure AD no painel **Alterar controle de acesso**, o serviço de gateway será reiniciado e você deverá atualizar o navegador. Você pode atualizar o acesso do usuário para o aplicativo do Azure AD do Windows Admin Center no portal do Azure a qualquer momento. 

Os usuários serão solicitados a entrar usando a identidade do Azure Active Directory quando tentarem acessar a URL do gateway do Windows Admin Center. Lembre-se de que os usuários também devem ser membros dos usuários locais no servidor de gateway para acessarem o Windows Admin Center. 

Usando a guia **Azure** das configurações gerais do Windows Admin Center, os usuários e os administradores podem exibir a conta conectada no momento, bem como sair dessa conta do Azure AD.

### <a name="conditional-access-and-multi-factor-authentication"></a>Acesso condicional e autenticação multifator

Um dos benefícios de usar o Azure AD como uma camada adicional de segurança para controlar o acesso ao gateway do Windows Admin Center é que você pode aproveitar os eficientes recursos de segurança do Azure AD, como acesso condicional e autenticação multifator. 

[Saiba mais sobre como configurar o acesso condicional com o Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## <a name="configure-single-sign-on"></a>Configurar logon único

**Logon único quando implantado como um serviço no Windows Server**

Quando você instala o Windows Admin Center no Windows 10, ele está pronto para usar o logon único. No entanto, se você pretende usar o Windows Admin Center no Windows Server, precisará configurar alguma forma de delegação do Kerberos em seu ambiente para poder usar o logon único. A delegação configura o computador do gateway como confiável para delegar ao nó de destino. 

Para configurar [Delegação restrita baseada em recursos](https://docs.microsoft.com/windows-server/security/kerberos/kerberos-constrained-delegation-overview) em seu ambiente, use o seguinte exemplo do PowerShell. Este exemplo mostra como você deve configurar um Windows Server [node01.contoso.com] para aceitar a delegação do gateway do Windows Admin Center [wac.contoso.com] no domínio contoso.com.

```powershell
Set-ADComputer -Identity (Get-ADComputer node01) -PrincipalsAllowedToDelegateToAccount (Get-ADComputer wac)
```

Para remover essa relação, execute o seguinte cmdlet:

```powershell
Set-ADComputer -Identity (Get-ADComputer node01) -PrincipalsAllowedToDelegateToAccount $null
```

## <a name="role-based-access-control"></a>Controle de acesso baseado em função

O controle de acesso baseado em função permite que você forneça aos usuários acesso limitado ao computador, em vez de torná-los administradores locais completos.
[Leia mais sobre o controle de acesso baseado em função e as funções disponíveis.](../plan/user-access-options.md#role-based-access-control)

A configuração do RBAC consiste em duas etapas: habilitar o suporte nos computadores de destino e atribuir usuários às funções relevantes.

> [!TIP]
> Verifique se você tem privilégios de administrador local nos computadores em que está configurando o suporte para o controle de acesso baseado em função.

### <a name="apply-role-based-access-control-to-a-single-machine"></a>Aplicar o controle de acesso baseado em função a um único computador

O modelo de implantação de computador único é ideal para ambientes simples com apenas alguns computadores a serem gerenciados.
Configurar um computador com suporte para controle de acesso baseado em função resultará nas seguintes alterações:

-   Os módulos do PowerShell com funções exigidas pelo Windows Admin Center serão instalados na unidade do sistema, em `C:\Program Files\WindowsPowerShell\Modules`. Todos os módulos começarão com **Microsoft.Sme**
-   A Desired State Configuration executará uma configuração única para configurar um ponto de extremidade de Administração Just Enough no computador chamado **Microsoft.SME.PowerShell**. Esse ponto de extremidade define as três funções usadas pelo Windows Admin Center e será executado como um administrador local temporário quando um usuário se conectar a ele.
-   Três novos grupos locais serão criados para controlar quais usuários recebem acesso a quais funções:
    -   Administradores do Windows Admin Center
    -   Administradores do Hyper-V do Windows Admin Center
    -   Leitores do Windows Admin Center

Para habilitar o suporte para o controle de acesso baseado em função em um único computador, siga estas etapas:

1.  Abra o Windows Admin Center e conecte-se ao computador que você deseja configurar com o controle de acesso baseado em função usando uma conta com privilégios de administrador local no computador de destino.
2.  Na ferramenta **Visão Geral**, clique em **Configurações** > **Controle de acesso baseado em função**.
3.  Clique em **Aplicar** na parte inferior da página para habilitar o suporte ao controle de acesso baseado em função no computador de destino. O processo de aplicativo envolve copiar scripts do PowerShell e invocar uma configuração (usando a Desired State Configuration do PowerShell) no computador de destino. O processo pode levar até 10 minutos para ser concluído e resultará na reinicialização do WinRM. Isso desconectará temporariamente os usuários do Windows Admin Center, do PowerShell e do WMI.
4.  Atualize a página para verificar o status do controle de acesso baseado em função. Quando estiver pronto para uso, o status será alterado para **Aplicado**.

Depois que a configuração for aplicada, você poderá atribuir usuários às funções:

1.  Abra a ferramenta **Usuários e Grupos Locais** e navegue até a guia **Grupos**.
2.  Selecione o grupo **Leitores do Windows Admin Center**.
3.  No painel *Detalhes* na parte inferior, clique em **Adicionar Usuário** e insira o nome de um usuário ou grupo de segurança que deve ter acesso somente leitura ao servidor por meio do Windows Admin Center. Os usuários e grupos podem vir do computador local ou do seu domínio do Active Directory.
4.  Repita as etapas 2-3 para os grupos de **Administradores do Hyper-V do Windows Admin Center** e **Administradores do Windows Admin Center**.

Você também pode preencher esses grupos de forma consistente em seu domínio configurando um objeto de Política de Grupo com a [Configuração de Política de Grupos Restritos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### <a name="apply-role-based-access-control-to-multiple-machines"></a>Aplicar o controle de acesso baseado em função a vários computadores

Em uma grande implantação corporativa, você pode usar suas ferramentas de automação existentes para enviar por push o recurso de controle de acesso baseado em função para seus computadores baixando o pacote de configuração do gateway do Windows Admin Center.
O pacote de configuração é feito para ser usado com a Desired State Configuration do PowerShell, mas você pode adaptá-lo para funcionar com sua solução de automação preferida.

#### <a name="download-the-role-based-access-control-configuration"></a>Baixar a configuração do controle de acesso baseado em função

Para baixar o pacote de configuração de controle de acesso baseado em função, você precisará ter acesso ao Windows Admin Center e ao prompt do PowerShell.

Se você estiver executando o gateway do Windows Admin Center no modo de serviço no Windows Server, use o comando a seguir para baixar o pacote de configuração.
Atualize o endereço do gateway com o correto para seu ambiente.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Se você estiver executando o gateway do Windows Admin Center em seu computador com Windows 10, execute o seguinte comando:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Ao expandir o arquivo zip, você verá a seguinte estrutura de pastas:

- InstallJeaFeatures.ps1
- JustEnoughAdministration (diretório)
- Módulos (diretório)
    - Microsoft.SME.\* (diretórios)
    - WindowsAdminCenter.Jea (diretório)

Para configurar o suporte para o controle de acesso baseado em função em um nó, você precisa executar as seguintes ações:

1.  Copie os módulos JustEnoughAdministration, Microsoft.SME.\* e WindowsAdminCenter.Jea para o diretório de módulo do PowerShell no computador de destino. Normalmente, está localizado em `C:\Program Files\WindowsPowerShell\Modules`.
2.  Atualize o arquivo **InstallJeaFeature.ps1** para corresponder à configuração desejada para o ponto de extremidade do RBAC.
3.  Execute InstallJeaFeature.ps1 para compilar o recurso de DSC.
4.  Implante sua configuração DSC em todos os seus computadores para aplicar a configuração.

A seção a seguir explica como fazer isso usando a comunicação remota do PowerShell.

#### <a name="deploy-on-multiple-machines"></a>Implantar em vários computadores

Para implantar a configuração baixada para vários computadores, você precisará atualizar o script **InstallJeaFeatures.ps1** para incluir os grupos de segurança apropriados para seu ambiente, copiar os arquivos para cada um dos computadores e invocar os scripts de configuração.
Você pode usar suas ferramentas de automação preferenciais para fazer isso, no entanto, este artigo se concentrará em uma abordagem pura baseada no PowerShell.

Por padrão, o script de configuração criará grupos de segurança locais no computador para controlar o acesso a cada uma das funções.
Isso é adequado para computadores ingressados no domínio e no grupo de trabalho, mas se você estiver implantando em um ambiente somente de domínio, talvez queira associar diretamente um grupo de segurança de domínio a cada função.
Para atualizar a configuração para usar grupos de segurança de domínio, abra **InstallJeaFeatures.ps1** e faça as seguintes alterações:

1.  Remova os três recursos de **Grupo** do arquivo:
    1.  "Group MS-Readers-Group"
    2.  "Group MS-Hyper-V-Administrators-Group"
    3.  "Group MS-Administrators-Group"
2.  Remova os três recursos de Grupo da propriedade **DependsOn** do JeaEndpoint
    1.  "[Group]MS-Readers-Group"
    2.  "[Group]MS-Hyper-V-Administrators-Group"
    3.  "[Group]MS-Administrators-Group"
3.  Altere os nomes de grupo na propriedade **RoleDefinitions** do JeaEndpoint para os grupos de segurança desejados. Por exemplo, se você tiver um grupo de segurança *CONTOSO\MyTrustedAdmins* que deve receber acesso à função Administradores do Windows Admin Center, altere `'$env:COMPUTERNAME\Windows Admin Center Administrators'` para `'CONTOSO\MyTrustedAdmins'`. As três cadeias de caracteres que você precisa atualizar são:
    1.  '$env:COMPUTERNAME\Windows Admin Center Administrators'
    2.  '$env:COMPUTERNAME\Windows Admin Center Hyper-V Administrators'
    3.  '$env:COMPUTERNAME\Windows Admin Center Readers'

> [!NOTE]
> Use grupos de segurança exclusivos para cada função. A configuração falhará se o mesmo grupo de segurança for atribuído a várias funções.

Em seguida, no final do arquivo **InstallJeaFeatures.ps1**, adicione as seguintes linhas do PowerShell à parte inferior do script:

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

Por fim, você pode copiar a pasta que contém os módulos, o recurso DSC e a configuração para cada nó de destino e executar o script **InstallJeaFeature.ps1**.
Para fazer isso remotamente de sua estação de trabalho de administração, você pode executar os seguintes comandos:

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
