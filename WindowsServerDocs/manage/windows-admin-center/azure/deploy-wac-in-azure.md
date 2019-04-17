---
title: Implantar um gateway do Windows Admin Center no Azure
description: Como implantar um gateway do Windows Admin Center no Azure
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: af766c2e0502d9fe633cae42d999db5cbffc32c8
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296846"
---
# Implantar o Windows Admin Center no Azure

## Implantar usando script

Você pode baixar [Implantar WACAzVM.ps1](https://aka.ms/deploy-wacazvm) será executado a partir do [Shell de nuvem do Azure](https://shell.azure.com) para configurar um gateway do Windows Admin Center no Azure. Esse script pode criar todo o ambiente, incluindo o grupo de recursos.

[Ir para as etapas de implantação manual](#deploy-manually-on-an-existing-azure-virtual-machine)

### Pré-requisitos

* Configure sua conta no [Shell de nuvem do Azure](https://shell.azure.com). Se esta for a primeira vez usando o Shell de nuvem, você deverá associar ou criar uma conta de armazenamento do Azure com o Shell de nuvem.
* Em um Shell de nuvem do **PowerShell** , navegue até o diretório: ```PS Azure:\> cd ~```
* Para carregar o ```Deploy-WACAzVM.ps1``` arquivo, arraste e solte-o na sua máquina local para qualquer lugar na janela do Shell de nuvem.

Se especificando seu próprio certificado:

* Carrega o certificado para [Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-whatis). Primeiro, crie um cofre de chave no Portal do Azure e carregar o certificado para o Cofre de chave. Como alternativa, você pode usar o Portal do Azure para gerar um certificado para você.

### Parâmetros de script

* **ResourceGroupName** - [String] Especifica o nome do grupo de recursos onde a VM será criada.

* **Nome** - [String] Especifica o nome da VM.

* **Credencial** - [PSCredential] Especifica as credenciais para a VM.

* **MsiPath** - [String] Especifica o caminho local do Windows Admin Center MSI ao implantar o Windows Admin Center em uma VM existente. Padrão é a versão do http://aka.ms/WACDownload se omitido.

* **VaultName** - [String] Especifica o nome do Cofre de chave que contém o certificado.

* **CertName** - [String] Especifica o nome do certificado a ser usado para instalação do MSI.

* **GenerateSslCert** - [Switch] verdadeiro se o MSI deve gerar um certificado autoassinado ssl assinado.

* **PortNumber** - [int] Especifica o número da porta ssl para o serviço do Windows Admin Center. O padrão é 443 se omitido.

* **OpenPorts** - [int []] Especifica as portas abertas para a VM.

* **Local** - [String] Especifica o local da VM.

* **Tamanho** - [String] Especifica o tamanho da VM. O padrão é "Standard_DS1_v2" Se omitido.

* **Imagem** - [String] Especifica a imagem da VM. O padrão é "Win2016Datacenter" Se omitido.

* **VirtualNetworkName** - [String] Especifica o nome da rede virtual para a VM.

* **SubnetName** - [String] Especifica o nome da sub-rede para a VM.

* **SecurityGroupName** - [String] Especifica o nome do grupo de segurança para a VM.

* **PublicIpAddressName** - [String] Especifica o nome do endereço IP público para a VM.

* **InstallWACOnly** - [opção] definida como True se WAC deve ser instalado em uma VM preexistente do Azure.

Há 2 opções diferentes para o MSI para implantar e o certificado usado para a instalação do MSI. MSI ou pode ser baixado da aka.ms/WACDownload ou, se a implantação de uma VM existente, o caminho do arquivo de um MSI localmente na VM pode ser fornecido. O certificado pode ser encontrado em qualquer uma das Key Vault Azure ou um certificado autoassinado será gerado pelo MSI.

### Exemplos de script

Primeiro, defina variáveis comuns necessárias para os parâmetros do script.

```PowerShell
$ResourceGroupName = "wac-rg1" 
$VirtualNetworkName = "wac-vnet"
$SecurityGroupName = "wac-nsg"
$SubnetName = "wac-subnet"
$VaultName = "wac-key-vault"
$CertName = "wac-cert"
$Location = "westus"
$PublicIpAddressName = "wac-public-ip"
$Size = "Standard_D4s_v3"
$Image = "Win2016Datacenter"
$Credential = Get-Credential
```

#### Exemplo 1: Use o script para implantar o gateway WAC em uma nova VM em um grupo de recursos e rede virtual novo. Use o MSI do aka.ms/WACDownload e um certificado autoassinado do MSI.

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm1"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### Exemplo 2: Igual #1, mas com um certificado da Azure Key Vault.

```PowerShell
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm2"
    Credential = $Credential
    VirtualNetworkName = $VirtualNetworkName
    SubnetName = $SubnetName
    VaultName = $VaultName
    CertName = $CertName
}
./Deploy-WACAzVM.ps1 @scriptParams
```

#### Exemplo 3: Usando um MSI local em uma VM existente para implantar WAC.

```PowerShell
$MsiPath = "C:\Users\<username>\Downloads\WindowsAdminCenter<version>.msi"
$scriptParams = @{
    ResourceGroupName = $ResourceGroupName
    Name = "wac-vm3"
    Credential = $Credential
    MsiPath = $MsiPath
    InstallWACOnly = $true
    GenerateSslCert = $true
}
./Deploy-WACAzVM.ps1 @scriptParams
```

### Requisitos para a VM que executa o gateway do Windows Admin Center

Porta 443 (HTTPS) deve estar aberta.
Usando as mesmas variáveis definidas para script, você pode usar o código a seguir no Shell de nuvem do Azure para atualizar o grupo de segurança de rede:

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### Requisitos para da VM do Azure gerenciado

Porta 5985 (WinRM por HTTP) deve ser aberta e ter um ouvinte de ativo.
Você pode usar o código a seguir no Shell de nuvem do Azure para atualizar os nós gerenciados. ```$ResourceGroupName``` e ```$Name``` usar as mesmas variáveis como o script de implantação, mas você precisará usar o ```$Credential``` específicos à VM que você está gerenciando.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## Implantar manualmente em uma máquina virtual existente do Azure

Antes de instalar o Windows Admin Center no seu gateway desejado VM, instalar um certificado SSL para comunicação HTTPS, ou você pode optar por usar um certificado autoassinado gerado pelo Windows Admin Center. No entanto, você receberá um aviso ao tentar se conectar usando um navegador, se você escolher a última opção. Você pode ignorar esse aviso no Edge, clicando em **Detalhes gt _ ir para a página da Web** ou, no Chrome, selecionando **avançados gt _ prosseguir para [página da Web]**. Recomendamos que você use apenas certificados autoassinados para ambientes de teste.

> [!NOTE]
> Essas instruções são para a instalação no Windows Server com experiência Desktop, não em uma instalação Server Core. 

1. [Baixe o Windows Admin Center](https://aka.ms/windowsadmincenter) em seu computador local.

2. Estabelecer uma conexão de área de trabalho remota à VM, em seguida, copie o MSI de sua máquina local e cole a VM.

3. Clique duas vezes o MSI para iniciar a instalação e siga as instruções no assistente. Lembre-se do seguinte:

   - Por padrão, o instalador usa a porta recomendada 443 (HTTPS). Se você quiser selecionar uma porta diferente, observe que você precisa abrir essa porta no firewall também. 

   - Se você já tiver instalado um certificado SSL na VM, verifique se você selecionar essa opção e insira a impressão digital.

4. Iniciar o serviço do Windows Admin Center (executar o programa/c: arquivos/Windows administrador Center/sme.exe)

[Saiba mais sobre como implantar o Windows Admin Center.](../deploy/install.md)

### Configure o gateway VM para habilitar o acesso de porta HTTPS: 

1. Navegue até sua VM no portal do Azure e selecione a **rede**. 

2. Selecione **Adicionar regra de porta de entrada** e **HTTPS** em **serviço**. 

> [!NOTE]
> Se você escolher uma porta diferente do padrão 443, escolha **personalizado** em serviço e insira a porta que você escolheu na etapa 3 em **intervalos de portas**. 

### Acessar um gateway do Windows Admin Center instalado em uma VM do Azure

Neste ponto, você deve ser capaz de acessar o Windows Admin Center em um navegador moderno (borda ou cromo) no computador local, navegando até o nome DNS do seu gateway VM. 

> [!NOTE]
> Se você selecionou uma porta diferente 443, você pode acessar o Windows Admin Center, navegando até https://\<DNS nome do seu VM\>:\<custom port\>

Quando você tentar acessar o Windows Admin Center, o navegador solicitará as credenciais acessar a máquina virtual no qual o Windows Admin Center está instalado. Aqui, você precisará inserir as credenciais que são os usuários locais ou o grupo de Administradores Local da máquina virtual. 

Para adicionar outras VMs a VNet, verifique se que está em execução no destino VMs WinRM executando o seguinte no PowerShell ou do prompt de comando no destino VM: `winrm quickconfig`

Se você ainda não tenha ingressado no domínio VM do Azure, a VM se comporta como um servidor no grupo de trabalho, portanto, você precisará considerar para [usar o Windows Admin Center em um grupo de trabalho](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).