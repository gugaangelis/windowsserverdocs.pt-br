---
title: Implantar um gateway do centro de administração do Windows no Azure
description: Como implantar um gateway do centro de administração do Windows no Azure
ms.technology: manage
ms.topic: article
author: jwwool
ms.author: jeffrew
ms.date: 04/12/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d0ebc957715f88898a9c14d2841d8b820f862a0d
ms.sourcegitcommit: f6490192d686f0a1e0c2ebe471f98e30105c0844
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2019
ms.locfileid: "70869143"
---
# <a name="deploy-windows-admin-center-in-azure"></a>Implantar o centro de administração do Windows no Azure

## <a name="deploy-using-script"></a>Implantar usando script

Você pode baixar [Deploy-WACAzVM. ps1](https://aka.ms/deploy-wacazvm) , que será executado de [Azure cloud Shell](https://shell.azure.com) para configurar um gateway do centro de administração do Windows no Azure. Esse script pode criar todo o ambiente, incluindo o grupo de recursos.

[Ir para etapas de implantação manual](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>Pré-requisitos

* Configure sua conta no [Azure cloud Shell](https://shell.azure.com). Se esta for a primeira vez que você usa Cloud Shell, você será solicitado a associar ou criar uma conta de armazenamento do Azure com Cloud Shell.
* Em um Cloud Shell do **PowerShell** , navegue até seu diretório base:```PS Azure:\> cd ~```
* Para carregar o ```Deploy-WACAzVM.ps1``` arquivo, arraste-o e solte-o do computador local para qualquer lugar na janela de Cloud Shell.

Se especificar seu próprio certificado:

* Carregue o certificado para [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Primeiro, crie um cofre de chaves no portal do Azure e, em seguida, carregue o certificado no cofre de chaves. Como alternativa, você pode usar o portal do Azure para gerar um certificado para você.

### <a name="script-parameters"></a>Parâmetros de script

* **ResourceGroupName** -[String] Especifica o nome do grupo de recursos em que a VM será criada.

* **Name** -[cadeia de caracteres] Especifica o nome da VM.

* **Credential** -[PSCredential] Especifica as credenciais para a VM.

* **MsiPath** -[cadeia de caracteres] Especifica o caminho local do MSI do centro de administração do Windows ao implantar o centro de administração do Windows em uma VM existente. O padrão é a versão de http://aka.ms/WACDownload se omitida.

* **Vaultname** -[cadeia de caracteres] Especifica o nome do cofre de chaves que contém o certificado.

* **CertName** -[cadeia de caracteres] Especifica o nome do certificado a ser usado para a instalação do MSI.

* **GenerateSslCert** -[opção] true se o MSI deve gerar um certificado SSL autoassinado.

* **PortNumber** -[int] Especifica o número da porta SSL para o serviço do centro de administração do Windows. O padrão é 443 se omitido.

* **Openports** -[int []] Especifica as portas abertas para a VM.

* **Location** -[cadeia de caracteres] Especifica o local da VM.

* **Size** -[cadeia de caracteres] Especifica o tamanho da VM. O padrão é "Standard_DS1_v2" se omitido.

* **Image** -[String] Especifica a imagem da VM. O padrão é "Win2016Datacenter" se omitido.

* **VirtualNetworkName** -[String] Especifica o nome da rede virtual para a VM.

* **SubnetName** -[cadeia de caracteres] Especifica o nome da sub-rede para a VM.

* **SecurityGroupName** -[cadeia de caracteres] Especifica o nome do grupo de segurança para a VM.

* **PublicIpAddressName** -[String] Especifica o nome do endereço IP público para a VM.

* **InstallWACOnly** -[switch] definido como true se WAC deve ser instalado em uma VM do Azure pré-existente.

Há duas opções diferentes para o MSI implantar e o certificado usado para a instalação do MSI. O MSI pode ser baixado de aka.ms/WACDownload ou, se estiver implantando em uma VM existente, o FilePath de um MSI localmente na VM pode ser fornecido. O certificado pode ser encontrado em um Azure Key Vault ou um certificado autoassinado será gerado pelo MSI.

### <a name="script-examples"></a>Exemplos de script

Primeiro, defina as variáveis comuns necessárias para os parâmetros do script.

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

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>Exemplo 1: Use o script para implantar o gateway WAC em uma nova VM em uma nova rede virtual e grupo de recursos. Use o MSI do aka.ms/WACDownload e um certificado autoassinado do MSI.

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

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>Exemplo 2: O mesmo que #1, mas usando um certificado de Azure Key Vault.

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

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>Exemplo 3: Usando um MSI local em uma VM existente para implantar o WAC.

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

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>Requisitos para a VM que executa o gateway do centro de administração do Windows

A porta 443 (HTTPS) deve estar aberta.
Usando as mesmas variáveis definidas para script, você pode usar o código abaixo em Azure Cloud Shell para atualizar o grupo de segurança de rede:

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>Requisitos para VMs gerenciadas do Azure

A porta 5985 (WinRM sobre HTTP) deve estar aberta e ter um ouvinte ativo.
Você pode usar o código abaixo em Azure Cloud Shell para atualizar os nós gerenciados. ```$ResourceGroupName```e ```$Name``` usar as mesmas variáveis que o script de implantação, mas será necessário usar o ```$Credential``` específico para a VM que você está gerenciando.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>Implantar manualmente em uma máquina virtual do Azure existente

Antes de instalar o centro de administração do Windows em sua VM de gateway desejada, instale um certificado SSL a ser usado para comunicação HTTPS ou você pode optar por usar um certificado autoassinado gerado pelo centro de administração do Windows. No entanto, você receberá um aviso ao tentar se conectar a partir de um navegador se escolher a última opção. Você pode ignorar esse aviso no Edge clicando em **detalhes > ir para a página da Web** ou, no Chrome, selecionando **> avançado vá para [página da Web]** . Recomendamos que você use somente certificados autoassinados para ambientes de teste.

> [!NOTE]
> Essas instruções são para instalação no Windows Server com experiência desktop, não em uma instalação Server Core. 

1. [Baixe o centro de administração do Windows](https://aka.ms/windowsadmincenter) no computador local.

2. Estabeleça uma conexão de área de trabalho remota com a VM e copie a MSI do computador local e cole-a na VM.

3. Clique duas vezes no MSI para iniciar a instalação e siga as instruções no assistente. Lembre-se do seguinte:

   - Por padrão, o instalador usa a porta recomendada 443 (HTTPS). Se você quiser selecionar uma porta diferente, observe que também precisará abrir essa porta no firewall. 

   - Se você já tiver instalado um certificado SSL na VM, certifique-se de selecionar essa opção e insira a impressão digital.

4. Iniciar o serviço do centro de administração do Windows (executar C:/Arquivos de programas/centro de administração do Windows/SME. exe)

[Saiba mais sobre a implantação do centro de administração do Windows.](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>Configure a VM do gateway para habilitar o acesso à porta HTTPS: 

1. Navegue até sua VM no portal do Azure e selecione **rede**. 

2. Selecione **Adicionar regra de porta de entrada** e selecione **https** em **serviço**. 

> [!NOTE]
> Se você escolher uma porta diferente do padrão 443, escolha **personalizado** em serviço e insira a porta escolhida na etapa 3 em intervalos de **porta**. 

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>Acessando um gateway do centro de administração do Windows instalado em uma VM do Azure

Neste ponto, você deve ser capaz de acessar o centro de administração do Windows de um navegador moderno (Edge ou Chrome) em seu computador local, navegando até o nome DNS da sua VM de gateway. 

> [!NOTE]
> Se você selecionou uma porta diferente de 443, poderá acessar o centro de administração do Windows navegando até https://\<nome DNS da sua VM\>:\<porta personalizada\>

Quando você tentar acessar o centro de administração do Windows, o navegador solicitará credenciais para acessar a máquina virtual na qual o centro de administração do Windows está instalado. Aqui, você precisará inserir as credenciais que estão no grupo usuários locais ou administradores locais da máquina virtual. 

Para adicionar outras VMs na VNet, verifique se o WinRM está em execução nas VMs de destino executando o seguinte no PowerShell ou no prompt de comando na VM de destino:`winrm quickconfig`

Se você não ingressou no domínio na VM do Azure, a VM se comporta como um servidor no grupo de trabalho, portanto, você precisará se certificar de [usar o centro de administração do Windows em um grupo de trabalho](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).