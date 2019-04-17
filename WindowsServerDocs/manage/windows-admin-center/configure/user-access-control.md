---
title: Configurando o controle de acesso de usuário e permissões
description: Saiba como configurar o controle de acesso de usuário e permissões usando o Active Directory ou Azure AD (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 03/19/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: b19657f4ce1a1a2cfb94f7234f07805ba0abd42c
ms.sourcegitcommit: 4961576f2891600ef9a760ca7df650d14332e057
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/09/2019
ms.locfileid: "9152041"
---
# Configurar permissões e controle de acesso do usuário

>Aplicável à: Windows Admin Center, a visualização do Windows Admin Center

Se você ainda não fez isto, familiarize-se com as [Opções de controle de acesso de usuário no Centro de administração do Windows](../plan/user-access-options.md)

>[!NOTE]
> Não há suporte para o acesso de grupo com base no Windows Admin Center em ambientes de grupo de trabalho ou em domínios não confiáveis.

## Definições de função de acesso do gateway

Há duas funções para acessar o serviço de gateway do Windows Admin Center:

**Os usuários de gateway** pode se conectar para o serviço de gateway do Windows Admin Center para gerenciar servidores por meio desse gateway, mas não podem alterar as permissões de acesso nem o mecanismo de autenticação usado para autenticar para o gateway.

**Os administradores de gateway** pode definir quem tem acesso, bem como os usuários são autenticados para o gateway. Somente administradores de gateway podem exibir e definir as configurações de acesso no Windows Admin Center. Administradores locais no computador de gateway sempre são administradores do serviço de gateway do Windows Admin Center.

> [!NOTE]
> Acesso ao gateway do não implica acesso aos servidores gerenciados visível pelo gateway. Para gerenciar um servidor de destino, o usuário conectado deve usar credenciais (seja por meio de seu credencial passado pelo Windows ou credenciais fornecidas na sessão do Windows Admin Center usando a ação de **Gerenciar como** ) que têm acesso administrativo para esse servidor de destino.

## Active Directory ou grupos do computador local

Por padrão, o Active Directory ou grupos de computadores locais são usados para controlar o acesso do gateway. Se você tiver um domínio do Active Directory, você pode gerenciar usuários de gateway e administrador acessar de dentro da interface do Windows Admin Center.

Na guia **usuários** , você pode controlar quem pode acessar o Windows Admin Center como um usuário de gateway. Por padrão, e se você não especificar um grupo de segurança, qualquer usuário que acessa a URL de gateway tem acesso. Depois que você adiciona um ou mais grupos de segurança para a lista de usuários, o acesso é restrito aos membros desses grupos.

Se você não usar um domínio do Active Directory em seu ambiente, o acesso é controlado pelo ```Users``` e ```Administrators``` grupos locais no computador de gateway do Windows Admin Center.

### Autenticação de cartão inteligente

Você pode impor a **autenticação de cartão inteligente** , especificando um grupo adicionais _necessários_ para grupos de segurança baseada em cartão inteligente. Depois que você tiver adicionado um grupo de segurança baseada em cartão inteligente, um usuário só pode acessar o serviço do Windows Admin Center se eles são membros de qualquer grupo de segurança e um grupo de cartão inteligente incluídos na lista de usuários.

Na guia **administradores** , você pode controlar quem pode acessar o Windows Admin Center como um administrador de gateway. O grupo de administradores local no computador sempre terá acesso de administrador completo e não pode ser removido da lista. Adicionando grupos de segurança, você deve dar membros de privilégios esses grupos para alterar as configurações de gateway do Windows Admin Center. A lista de administradores oferece suporte à autenticação de cartão inteligente da mesma maneira como a lista de usuários: com a condição de e para um grupo de segurança e um grupo de cartão inteligente.

## Azure Active Directory

Se sua organização usa o Azure Active Directory (Azure AD), você pode optar por adicionar uma camada **adicional** de segurança para o Windows Admin Center ao exigir autenticação do Azure AD para acessar o gateway. Para acessar o Windows Admin Center, do usuário **conta do Windows** também deve ter acesso ao servidor de gateway (mesmo se a autenticação do Azure AD é usada). Quando você usa o Azure AD, você vai gerenciar permissões de acesso de usuário e do administrador do Windows Admin Center do Portal do Azure, em vez de dentro a interface do usuário do Windows Admin Center.

### Acessando o Windows Admin Center quando a autenticação do Azure AD está habilitada

Dependendo do navegador usado, alguns usuários ao acessar o Windows Admin Center com autenticação do Azure AD configurada receberá um adicionais prompt **do navegador** onde eles precisam fornecer seus Windows as credenciais de conta para o computador no qual Windows Admin Center está instalado. Depois de inserir essas informações, os usuários receberão a solicitação de autenticação adicional do Azure Active Directory, que requer as credenciais de uma conta do Azure que recebeu acesso no aplicativo do Azure AD no Azure.

> [!NOTE]
> Usuários que Windows conta tem **direitos de administrador** com a vontade de computador de gateway não será solicitado a autenticação do Azure AD.

### Configurando a autenticação do Azure Active Directory para Windows Admin Center Preview

Vá para **configurações**do Windows Admin Center > **acessar** e usar o botão de alternância para ativar "usar o Azure Active Directory para adicionar uma camada de segurança para o gateway". Se você não registrou o gateway no Azure, você será orientado para fazer isso no momento.

Por padrão, todos os membros do locatário do Azure AD têm acesso de usuário para o serviço de gateway do Windows Admin Center. Somente administradores locais no computador de gateway tem acesso de administrador para o gateway do Windows Admin Center. Observe que os direitos de administradores locais no computador de gateway não podem ser restritos - administradores locais podem fazer nada, independentemente se o Azure AD é usado para autenticação.

Se você quiser que os usuários oferecem específicas do Azure AD ou usuário de gateway grupos ou gateway de acesso de administrador para o serviço do Windows Admin Center, você deve fazer o seguinte:

1.  Vá para o seu aplicativo do Azure AD do Windows Admin Center no portal do Azure usando o hiperlink fornecido nas configurações de acesso. Observe que este hiperlink só está disponível quando a autenticação do Azure Active Directory está habilitada. 
    -   Você também pode encontrar seu aplicativo no portal do Azure indo para o **Azure Active Directory** > **aplicativos corporativos** > **todos os aplicativos** e pesquisa **WindowsAdminCenter** (o aplicativo do Azure AD será nomeado WindowsAdminCenter-<gateway name>). Se você não obter resultados de pesquisa, certifique-se de **Mostrar** for definido como **todos os aplicativos**, o **status do aplicativo** é definida como **qualquer** e clique em Aplicar, em seguida, tente a pesquisa. Depois de localizar o aplicativo, vá para **usuários e grupos**
2.  Na guia Propriedades, defina a **atribuição de usuário necessária** como Sim.
    Depois de fazer isso, somente os membros listados na guia **usuários e grupos** será capazes de acessar o gateway do Windows Admin Center.
3.  Na guia grupos e os usuários, selecione **Adicionar usuário**. Você deve atribuir um usuário de gateway ou a função de administrador do gateway para cada usuário/grupo adicionado.

Depois que você ativar a autenticação do Azure AD, o serviço de gateway é reiniciado e você deve atualizar seu navegador. Você pode atualizar o acesso do usuário para o aplicativo do Azure AD EA no portal do Azure a qualquer momento.

Os usuários serão solicitados a entrar usando sua identidade do Azure Active Directory quando tentam acessar a URL de gateway do Windows Admin Center. Lembre-se de que os usuários também devem ser um membro dos usuários locais no servidor de gateway para acessar o Windows Admin Center.

Os usuários e administradores podem exibir a conta conectado no momento e saída, bem como do Azure essa AD configurações da conta a partir da guia de **conta** do Windows Admin Center.

### Configurando a autenticação do Azure Active Directory para o Windows Admin Center

[Para configurar a autenticação do Azure AD, primeiro você deve se registrar seu gateway com o Azure](azure-integration.md) (você só precisa fazer isso vez para o gateway do Windows Admin Center). Esta etapa cria um aplicativo do Azure AD no qual você pode gerenciar usuários de gateway e acesso de administrador do gateway.

Se você quiser que os usuários oferecem específicas do Azure AD ou usuário de gateway grupos ou gateway de acesso de administrador para o serviço do Windows Admin Center, você deve fazer o seguinte:

1.  Vá para o seu aplicativo do Azure AD EA no portal do Azure. 
    -   Quando você clica **alterar o controle de acesso** e, em seguida, selecione **Azure Active Directory** das configurações de acesso do Windows Admin Center, você pode usar o hiperlink fornecido na interface do usuário para acessar o aplicativo do Azure AD no portal do Azure. Este hiperlink também está disponível nas configurações de acesso, depois clique em Salvar e selecionou do Azure AD como seu provedor de identidade de controle de acesso.
    -   Você também pode encontrar seu aplicativo no portal do Azure indo para o **Azure Active Directory** > **aplicativos corporativos** > **todos os aplicativos** e pesquisa **EA** (o aplicativo do Azure AD será nomeado EA -<gateway>). Se você não obter resultados de pesquisa, certifique-se de **Mostrar** for definido como **todos os aplicativos**, o **status do aplicativo** é definida como **qualquer** e clique em Aplicar, em seguida, tente a pesquisa. Depois de localizar o aplicativo, vá para **usuários e grupos**
2.  Na guia Propriedades, defina a **atribuição de usuário necessária** como Sim.
    Depois de fazer isso, somente os membros listados na guia **usuários e grupos** será capazes de acessar o gateway do Windows Admin Center.
3.  Na guia grupos e os usuários, selecione **Adicionar usuário**. Você deve atribuir um usuário de gateway ou a função de administrador do gateway para cada usuário/grupo adicionado.

Depois que você salvar o controle de acesso do Azure AD no painel de **controle de acesso de alteração** , o serviço de gateway é reiniciado e você deve atualizar seu navegador. Você pode atualizar o acesso do usuário para o aplicativo do Azure AD do Windows Admin Center no portal do Azure a qualquer momento. 

Os usuários serão solicitados a entrar usando sua identidade do Azure Active Directory quando tentam acessar a URL de gateway do Windows Admin Center. Lembre-se de que os usuários também devem ser um membro dos usuários locais no servidor de gateway para acessar o Windows Admin Center. 

Usando a guia **Azure** das configurações gerais do Windows Admin Center, usuários e administradores podem exibir a conta conectado no momento e também como saída dessa conta do Azure AD.

### Acesso condicional e a autenticação multifator

Um dos benefícios do uso do Azure AD como uma camada adicional de segurança para controlar o acesso ao gateway do Windows Admin Center é que você pode aproveitar os recursos de segurança avançados do Azure AD como o acesso condicional e a autenticação multifator. 

[Saiba mais sobre como configurar o acesso condicional com o Azure Active Directory.](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal-get-started)

## Configurar o logon único

**Logon único quando implantado como um serviço no Windows Server**

Quando você instala o Windows Admin Center no Windows 10, ele estará pronto para usar o logon único. No entanto, se você pretende usar o Windows Admin Center no Windows Server, você precisará configurar alguma forma de delegação Kerberos em seu ambiente antes de usar logon único. A delegação configura o computador de gateway como confiável delegado para o nó de destino. 

Para configurar a [delegação restrita de baseado em recursos](http://windowsitpro.com/security/how-windows-server-2012-eases-pain-kerberos-constrained-delegation-part-1) em seu ambiente, execute os seguintes cmdlets do PowerShell. (Esteja ciente de que isso requer um controlador de domínio que executam o Windows Server 2012 ou posterior).

```powershell
     $gateway = "WindowsAdminCenterGW" # Machine where Windows Admin Center is installed
     $node = "ManagedNode" # Machine that you want to manage
     $gatewayObject = Get-ADComputer -Identity $gateway
     $nodeObject = Get-ADComputer -Identity $node
     Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $gatewayObject
```

Neste exemplo, o gateway do Windows Admin Center está instalado no servidor **WindowsAdminCenterGW**, e o nome do nó de destino é **ManagedNode**.

Para remover essa relação, execute o seguinte cmdlet:

```powershell
Set-ADComputer -Identity $nodeObject -PrincipalsAllowedToDelegateToAccount $null
```

## Controle de acesso baseado em função

Controle de acesso baseado em função permite que você forneça aos usuários com acesso limitado ao computador, em vez de fazer administradores locais completo-los.
[Leia mais sobre o controle de acesso baseado em função e as funções disponíveis.](../plan/user-access-options.md#role-based-access-control)

Configuração de RBAC consiste em 2 etapas: ativação do suporte para os computadores de destino e atribuir usuários às funções relevantes.

> [!TIP]
> Verifique se que você tem privilégios de administrador local nos computadores em que você está configurando o suporte para controle de acesso baseado em função.

### Aplicar o controle de acesso baseado em função para um único computador

O modelo de implantação única máquina é ideal para ambientes simples com apenas alguns computadores para gerenciar.
Configurar um computador com suporte para controle de acesso baseado em função resultará nas seguintes alterações:
-   Módulos do PowerShell com funções exigidos pelo Windows Admin Center serão instalados na unidade do sistema, em `C:\Program Files\WindowsPowerShell\Modules`. Todos os módulos serão iniciado com **Microsoft.Sme**
-   Configuração de estado desejado será executada uma única configuração para configurar um ponto de extremidade de administração Just enough no computador, chamado **Microsoft.Sme.PowerShell**. Esse ponto de extremidade define as funções de 3 usadas pelo Windows Admin Center e será executado como um administrador local temporário quando um usuário se conecta a ele.
-   3 novos grupos locais serão criados para controlar quais usuários recebem acesso a quais funções:
    -   Administradores do Windows Admin Center
    -   Administradores do Hyper-V do Windows Admin Center
    -   Leitores de Windows Admin Center

Para habilitar o suporte para controle de acesso baseado em função em um único computador, siga estas etapas:

1.  Abra o Windows Admin Center e conectar-se ao computador que você deseja configurar com controle de acesso baseado em função usando uma conta com privilégios de administrador local no computador de destino.
2.  Na ferramenta de **Visão geral** , clique em **configurações** > **controle de acesso baseado em função**.
3.  Clique em **Aplicar** na parte inferior da página para habilitar o suporte para controle de acesso baseado em função no computador de destino. O processo envolve copiar scripts do PowerShell e invocar uma configuração (usando a configuração de estado desejado do PowerShell) no computador de destino. Pode levar até 10 minutos para ser concluída e isso resultará em WinRM reiniciar. Isso desconectará temporariamente os usuários do Windows Admin Center, PowerShell e WMI.
4.  Atualize a página para verificar o status de controle de acesso baseado em função. Quando estiver pronto para uso, o status será alterado para **aplicada**.

Depois que a configuração for aplicada, você pode atribuir usuários às funções:

1.  Abra a ferramenta de **usuários e grupos locais** e navegue até a guia **grupos** .
2.  Selecione o grupo do **Windows Admin Center leitores** .
3.  No painel de *detalhes* na parte inferior, clique em **Adicionar usuário** e insira o nome de um usuário ou grupo de segurança que deve ter acesso somente leitura ao servidor por meio do Windows Admin Center. Os usuários e grupos podem vir de máquina local ou seu domínio do Active Directory.
4.  Repita as etapas 2 e 3 para os grupos de **Administradores do Hyper-V do Windows Admin Center** e **Os administradores do Windows Admin Center** .

Você também pode preencher esses grupos consistentemente em seu domínio, configurando um objeto de política de grupo com a [Configuração de política de grupos restritos](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2003/cc756802%28v=ws.10%29).

### Aplicar o controle de acesso baseado em função em vários computadores

Em uma grande implantação corporativa, você pode usar suas ferramentas de automação para enviar o recurso de controle de acesso baseado em função à seus computadores baixando o pacote de configuração do gateway do Windows Admin Center.
O pacote de configuração foi projetado para ser usado com a configuração de estado desejado do PowerShell, mas você pode adaptá-la para funcionar com a solução de automação preferencial.

#### Baixar a configuração de controle de acesso baseado em função

Para baixar o pacote de configuração de controle de acesso baseado em função, você precisará ter acesso ao centro de administração do Windows e um prompt do PowerShell.

Se você estiver executando o gateway do Windows Admin Center no modo de serviço no Windows Server, use o seguinte comando para baixar o pacote de configuração.
Não se esqueça de atualizar o endereço do gateway com uma correta para seu ambiente.

```powershell
$WindowsAdminCenterGateway = 'https://windowsadmincenter.contoso.com'
Invoke-RestMethod -Uri "$WindowsAdminCenterGateway/api/nodes/all/features/jea/endpoint/export" -Method POST -UseDefaultCredentials -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Se você estiver executando o gateway do Windows Admin Center no seu computador Windows 10, execute o seguinte comando em vez disso:

```powershell
$cert = Get-ChildItem Cert:\CurrentUser\My | Where-Object Subject -eq 'CN=Windows Admin Center Client' | Select-Object -First 1
Invoke-RestMethod -Uri "https://localhost:6516/api/nodes/all/features/jea/endpoint/export" -Method POST -Certificate $cert -OutFile "~\Desktop\WindowsAdminCenter_RBAC.zip"
```

Quando você expande o arquivo zip, você verá a seguinte estrutura de pastas:
- InstallJeaFeatures.ps1
- JustEnoughAdministration (diretório)
- Módulos (diretório)
    - Microsoft.SME.\* (diretórios)
    - WindowsAdminCenter.Jea (diretório)

Para configurar o suporte para controle de acesso baseado em função em um nó, você precisa executar as seguintes ações:
1.  Copie os módulos JustEnoughAdministration, Microsoft.SME.\* e WindowsAdminCenter.Jea para o diretório do módulo do PowerShell no computador de destino. Normalmente, isso está localizado em `C:\Program Files\WindowsPowerShell\Modules`.
2.  Atualize o arquivo **InstallJeaFeature.ps1** para corresponder a configuração desejada para o ponto de extremidade RBAC.
3.  Execute InstallJeaFeature.ps1 para compilar o recurso de DSC.
4.  Implante a configuração de DSC em todas as máquinas para aplicar a configuração.

A seção a seguir explica como fazer isso usando a comunicação remota do PowerShell.

#### Implantar em vários computadores

Para implantar a configuração você baixada em vários computadores, você precisa atualizar o script **InstallJeaFeatures.ps1** para incluir os grupos de segurança apropriados para seu ambiente, copie os arquivos para cada um dos seus computadores e invocar o scripts de configuração.
Você pode usar suas ferramentas de automação preferencial para fazer isso, no entanto, este artigo se concentrará em uma abordagem essencialmente baseado no PowerShell.

Por padrão, o script de configuração criará grupos de segurança local no computador para controlar o acesso para cada uma das funções.
Isso é adequado para o grupo de trabalho e domínio ingressou máquinas, mas se você estiver implantando em um ambiente de domínio somente, você pode querer diretamente associar um grupo de segurança do domínio cada função.
Para atualizar a configuração para usar grupos de segurança do domínio, abra **InstallJeaFeatures.ps1** e faça as seguintes alterações:

1.  Remova os recursos de **grupo** 3 do arquivo:
    1.  "Grupo MS-leitores-grupo"
    2.  "Grupo MS-Hyper-V-os administradores de TI-grupo"
    3.  "Grupo de administradores MS de grupo"
2.  Remova os recursos de grupo 3 da propriedade JeaEndpoint **DependsOn**
    1.  "[Grupo] MS-leitores-grupo"
    2.  "[Grupo] MS-Hyper-V--grupo Administradores"
    3.  "[Grupo] MS-os administradores de TI-grupo"
3.  Altere os nomes de grupo na propriedade JeaEndpoint **RoleDefinitions** aos grupos de segurança desejado. Por exemplo, se você tiver um grupo de segurança *CONTOSO\MyTrustedAdmins* que deve ser o acesso atribuído à função de administradores do Windows Admin Center, alterar `'$env:COMPUTERNAME\Windows Admin Center Administrators'` para `'CONTOSO\MyTrustedAdmins'`. As três cadeias de caracteres que você precisa atualizar são:
    1.  ' $env: dos administradores do COMPUTERNAME\Windows Admin Center
    2.  ' $env: dos administradores do Hyper-V COMPUTERNAME\Windows Admin Center
    3.  ' $env: dos leitores de COMPUTERNAME\Windows Admin Center

> [!NOTE]
> Certifique-se de usar grupos de segurança exclusivo para cada função. Configuração falhará se o mesmo grupo de segurança é atribuído a várias funções.

Em seguida, no final do arquivo **InstallJeaFeatures.ps1** , adicione as seguintes linhas do PowerShell para a parte inferior do script:

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

Por fim, você pode copiar a pasta que contém os módulos, recursos de DSC e configuração para cada nó de destino e execute o script **InstallJeaFeature.ps1** .
Para fazer isso remotamente em sua estação de trabalho de administrador, você pode executar os seguintes comandos:

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
