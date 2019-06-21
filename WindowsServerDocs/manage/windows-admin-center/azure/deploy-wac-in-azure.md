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
ms.openlocfilehash: a3fa1838096d910505faf9a2c5bd819b3a256fe2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280417"
---
# <a name="deploy-windows-admin-center-in-azure"></a>Implantar o Windows Admin Center no Azure

## <a name="deploy-using-script"></a>Implantar usando o script

Você pode baixar [Deploy WACAzVM.ps1](https://aka.ms/deploy-wacazvm) será executado a partir [Azure Cloud Shell](https://shell.azure.com) para configurar um gateway do Windows Admin Center no Azure. Esse script pode criar todo o ambiente, incluindo o grupo de recursos.

[Saltar para etapas de implantação manual](#deploy-manually-on-an-existing-azure-virtual-machine)

### <a name="prerequisites"></a>Pré-requisitos

* Configurar sua conta no [Azure Cloud Shell](https://shell.azure.com). Se essa for sua primeira vez usando o Cloud Shell, você deverá associar ou criar uma conta de armazenamento do Azure com o Cloud Shell.
* Em um **PowerShell** Cloud Shell, navegue até seu diretório base: ```PS Azure:\> cd ~```
* Para carregar o ```Deploy-WACAzVM.ps1``` de arquivo, arraste e solte-o em seu computador local em qualquer lugar na janela do Cloud Shell.

Se especificar seu próprio certificado:

* Carregar o certificado [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-whatis). Primeiro, crie um cofre de chaves no Portal do Azure e carregar o certificado no cofre de chaves. Como alternativa, você pode usar o Portal do Azure para gerar um certificado para você.

### <a name="script-parameters"></a>Parâmetros do script

* **ResourceGroupName** -[String] Especifica o nome do grupo de recursos onde a VM será criada.

* **Nome** -[String] Especifica o nome da VM.

* **Credencial** -[PSCredential] Especifica as credenciais para a VM.

* **MsiPath** -[String] Especifica o caminho local do Windows Admin Center MSI durante a implantação do Windows Admin Center em uma VM existente. O padrão é a versão de http://aka.ms/WACDownload se ele for omitido.

* **VaultName** -[String] Especifica o nome do Cofre de chaves que contém o certificado.

* **CertName** -[String] Especifica o nome do certificado a ser usado para a instalação do MSI.

* **GenerateSslCert** -[Switch] True se o MSI deve gerar um certificado ssl assinado self.

* **PortNumber** -[int] Especifica o número da porta ssl para o serviço Windows Admin Center. O padrão é 443 se ele for omitido.

* **OpenPorts** -[int []] Especifica as portas abertas para a VM.

* **Local** -[String] Especifica o local da VM.

* **Tamanho** -[String] Especifica o tamanho da VM. O padrão é "Standard_DS1_v2" se ele for omitido.

* **Imagem** -[String] Especifica a imagem da VM. O padrão é "Win2016Datacenter" se ele for omitido.

* **VirtualNetworkName** -[String] Especifica o nome da rede virtual para a VM.

* **SubnetName** -[String] Especifica o nome da sub-rede para a VM.

* **SecurityGroupName** -[String] Especifica o nome do grupo de segurança para a VM.

* **PublicIpAddressName** -[String] Especifica o nome do endereço IP público da VM.

* **InstallWACOnly** -[Switch] definido como True se WAC deve ser instalado em uma VM do Azure já existente.

Há 2 opções diferentes para o MSI para implantar e o certificado usado para a instalação do MSI. O MSI também pode ser baixado do aka.ms/WACDownload ou, se implantar uma VM existente, o caminho do arquivo de um MSI localmente na VM pode ser fornecido. O certificado pode ser encontrado em um cofre de chaves do Azure ou um certificado autoassinado será gerado pelo MSI.

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

#### <a name="example-1-use-the-script-to-deploy-wac-gateway-on-a-new-vm-in-a-new-virtual-network-and-resource-group-use-the-msi-from-akamswacdownload-and-a-self-signed-cert-from-the-msi"></a>Exemplo 1: Use o script para implantar o gateway WAC em uma nova VM em um novo grupo de recursos e rede de virtual. Use o MSI de aka.ms/WACDownload e um certificado autoassinado do MSI.

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

#### <a name="example-2-same-as-1-but-using-a-certificate-from-azure-key-vault"></a>Exemplo 2: Mesmo que o #1, mas usando um certificado do Azure Key Vault.

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

#### <a name="example-3-using-a-local-msi-on-an-existing-vm-to-deploy-wac"></a>Exemplo 3: Usando um MSI local em uma VM existente para implantar WAC.

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

### <a name="requirements-for-vm-running-the-windows-admin-center-gateway"></a>Requisitos de VM que executa o gateway do Windows Admin Center

A porta 443 (HTTPS) deve estar aberta.
Usando as mesmas variáveis definidas para o script, você pode usar o código a seguir no Azure Cloud Shell para atualizar o grupo de segurança de rede:

```powershell
$nsg = Get-AzNetworkSecurityGroup -Name $SecurityGroupName -ResourceGroupName $ResourceGroupName
$newNSG = Add-AzNetworkSecurityRuleConfig -NetworkSecurityGroup $nsg -Name ssl-rule -Description "Allow SSL" -Access Allow -Protocol Tcp -Direction Inbound -Priority 100 -SourceAddressPrefix Internet -SourcePortRange * -DestinationAddressPrefix * -DestinationPortRange 443
Set-AzNetworkSecurityGroup -NetworkSecurityGroup $newNSG
```

### <a name="requirements-for-managed-azure-vms"></a>Requisitos para VM do Azure gerenciado

Porta 5985 (WinRM sobre HTTP) deve estar aberta e tem um ouvinte ativo.
Você pode usar o código a seguir no Azure Cloud Shell para atualizar os nós gerenciados. ```$ResourceGroupName``` e ```$Name``` usar as mesmas variáveis como o script de implantação, mas você precisará usar o ```$Credential``` específico para a máquina virtual que você está gerenciando.

```powershell
Enable-AzVMPSRemoting -ResourceGroupName $ResourceGroupName -Name $Name
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any} -Credential $Credential
Invoke-AzVMCommand -ResourceGroupName $ResourceGroupName -Name $Name -ScriptBlock {winrm create winrm/config/Listener?Address=*+Transport=HTTP} -Credential $Credential
```

## <a name="deploy-manually-on-an-existing-azure-virtual-machine"></a>Implantar manualmente em uma máquina virtual do Azure existente

Antes de instalar o Windows Admin Center em seu gateway desejado da VM, instale um certificado SSL a ser usado para comunicação HTTPS, ou você pode optar por usar um certificado autoassinado gerado pelo Windows Admin Center. No entanto, você receberá um aviso ao tentar se conectar de um navegador, se você escolher a última opção. Você pode ignorar este aviso no Edge, clicando em **detalhes > Ir para a página da Web** ou, no Chrome, selecionando **Avançado > vá para [página da Web]** . É recomendável que você use somente os certificados autoassinados para ambientes de teste.

> [!NOTE]
> Essas instruções são para a instalação no Windows Server com experiência Desktop e não em uma instalação Server Core. 

1. [Download Windows Admin Center](https://aka.ms/windowsadmincenter) em seu computador local.

2. Estabelecer uma conexão de área de trabalho remota à VM, em seguida, copie o MSI em seu computador local e cole na VM.

3. Clique duas vezes no MSI para iniciar a instalação e siga as instruções no assistente. Esteja ciente das seguintes opções:

   - Por padrão, o instalador usa a porta recomendada 443 (HTTPS). Se você deseja selecionar uma porta diferente, observe que você precisa abrir essa porta no firewall também. 

   - Se você já tiver instalado um certificado SSL na VM, verifique se você selecionar essa opção e digite a impressão digital.

4. Iniciar o serviço Windows Admin Center (executar o programa de c: / Windows/arquivos Admin Center/sme.exe)

[Saiba mais sobre a implantação do Windows Admin Center.](../deploy/install.md)

### <a name="configure-the-gateway-vm-to-enable-https-port-access"></a>Configure o gateway de VM para habilitar o acesso de porta HTTPS: 

1. Navegue até sua VM no portal do Azure e selecione **rede**. 

2. Selecione **Adicionar regra de porta de entrada** e selecione **HTTPS** sob **serviço**. 

> [!NOTE]
> Se você escolher uma porta diferente do padrão de 443, escolha **personalizado** no serviço e insira a porta que você escolheu na etapa 3 abaixo **intervalos de porta**. 

### <a name="accessing-a-windows-admin-center-gateway-installed-on-an-azure-vm"></a>Acessando um gateway do Windows Admin Center instalado em uma VM do Azure

Neste ponto, você deve ser capaz de acessar o Windows Admin Center em um navegador moderno (Edge ou Chrome) no computador local, navegando até o nome DNS do seu gateway de VM. 

> [!NOTE]
> Se você tiver selecionado uma porta diferente de 443, você pode acessar o Windows Admin Center, navegando até https://\<nome DNS da sua VM\>:\<porta personalizada\>

Quando você tenta acessar o Windows Admin Center, o navegador solicitará as credenciais acessar a máquina virtual em que o Windows Admin Center está instalado. Aqui, você precisará inserir credenciais que estão no grupo de administradores locais da máquina virtual ou usuários locais. 

Para adicionar outras VMs na rede virtual, verifique se que o WinRM está em execução no destino VMs executando o seguinte no prompt de comando ou do PowerShell na VM de destino: `winrm quickconfig`

Se você ainda não tiver ingressado no domínio na VM do Azure, a VM se comporta como um servidor no grupo de trabalho, portanto, você precisará certificar-se de que você é responsável [usando o Windows Admin Center em um grupo de trabalho](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).