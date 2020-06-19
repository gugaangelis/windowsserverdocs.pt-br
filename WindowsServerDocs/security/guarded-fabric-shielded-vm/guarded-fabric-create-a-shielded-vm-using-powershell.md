---
title: Criar uma VM blindada usando o PowerShell
ms.prod: windows-server
ms.topic: article
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.technology: security-guarded-fabric
ms.date: 09/25/2019
ms.openlocfilehash: abc1a2af7353bd85e0ae7ac55debc36d63d1782f
ms.sourcegitcommit: fe89b8001ad664b3618708b013490de93501db05
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84942285"
---
# <a name="create-a-shielded-vm-using-powershell"></a>Criar uma VM blindada usando o PowerShell

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Em produção, normalmente você usaria um Gerenciador de malha (por exemplo, o VMM) para implantar VMs blindadas. No entanto, as etapas ilustradas abaixo permitem que você implante e valide todo o cenário sem um Gerenciador de malha.

Resumindo, você criará um disco de modelo, um arquivo de dados de blindagem, um arquivo de resposta de instalação autônoma e outros artefatos de segurança em qualquer computador e, em seguida, copiará esses arquivos para um host protegido e provisionar a VM blindada.

## <a name="create-a-signed-template-disk"></a>Criar um disco de modelo assinado

Para criar uma nova VM blindada, primeiro você precisa de um disco de modelo de VM blindado que é previamente criptografado com seu volume de so (ou partições raiz e de inicialização no Linux) assinados.
Siga os links abaixo para obter mais informações sobre como criar um disco de modelo.

- [Preparar um disco de modelo do Windows](guarded-fabric-create-a-shielded-vm-template.md)
- [Preparar um disco de modelo do Linux](guarded-fabric-create-a-linux-shielded-vm-template.md)

Você também precisará de uma cópia do catálogo de assinatura de volume do disco para criar o arquivo de dados de blindagem.
Para salvar esse arquivo, execute o seguinte comando no computador em que você criou o disco de modelo:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>Baixar metadados do guardião

Para cada uma das malhas de virtualização em que você deseja executar sua VM blindada, você precisará obter os metadados do guardião para os clusters HGS das malhas.
Seu provedor de hospedagem deve ser capaz de fornecer essas informações para você.

Se você estiver em um ambiente corporativo e puder se comunicar com o servidor HGS, os metadados do guardião estarão disponíveis em *http:// \<HGSCLUSTERNAME\> /KeyProtection/Service/Metadata/2014-07/metadata.xml*

## <a name="create-shielding-data-pdk-file"></a>Criar arquivo de dados de blindagem (PDK)

Os dados de blindagem são criados e possuídos por proprietários de VM de locatário e contêm segredos necessários para criar VMs blindadas que devem ser protegidas do administrador de malha, como a senha de administrador da VM blindada.
Os dados de blindagem são criptografados de modo que apenas os servidores e locatários HGS possam descriptografá-los.
Uma vez criado pelo proprietário do locatário/VM, o arquivo PDK resultante deve ser copiado para a malha protegida.
Para obter mais informações, consulte [o que são os dados de blindagem e por que ele é necessário?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

Além disso, você precisará de um arquivo de resposta de instalação autônoma (unattend.xml para Windows, varia para Linux). Consulte [criar um arquivo de resposta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file) para obter diretrizes sobre o que incluir no arquivo de resposta.

Execute os seguintes cmdlets em um computador com o Ferramentas de Administração de Servidor Remoto para VMs blindadas instaladas.
Se você estiver criando um PDK para uma VM do Linux, deverá fazer isso em um servidor que executa o Windows Server, versão 1709 ou posterior.

 
```powershell
# Create owner certificate, don't lose this!
# The certificate is stored at Cert:\LocalMachine\Shielded VM Local Certificates
$Owner = New-HgsGuardian –Name 'Owner' –GenerateCertificates
 
# Import the HGS guardian for each fabric you want to run your shielded VM
$Guardian = Import-HgsGuardian -Path C:\HGSGuardian.xml -Name 'TestFabric'
 
# Create the PDK file
# The "Policy" parameter describes whether the admin can see the VM's console or not
# Use "EncryptionSupported" if you are testing out shielded VMs and want to debug any issues during the specialization process
New-ShieldingDataFile -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Owner $Owner –Guardian $guardian –VolumeIDQualifier (New-VolumeIDQualifier -VolumeSignatureCatalogFilePath 'C:\temp\MyTemplateDiskCatalog.vsc' -VersionRule Equals) -WindowsUnattendFile 'C:\unattend.xml' -Policy Shielded
```
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>Provisionar VM blindada em um host protegido
Em um host que esteja executando o Windows Server 2016, você pode monitorar a VM a ser desligada para sinalizar a conclusão da tarefa de provisionamento e consultar os logs de eventos do Hyper-V para obter informações de erro se o provisionamento não for bem-sucedido.

Você também pode criar um novo disco de modelo toda vez antes de criar uma nova VM blindada.

Copie o arquivo de disco do modelo (ServerOS. vhdx) e o arquivo PDK (contoso. PDK) para o host protegido para se preparar para a implantação.

No host protegido, instale o módulo do PowerShell de ferramentas de malha protegida, que contém o cmdlet New-ShieldedVM para simplificar o processo de provisionamento. Se o host protegido tiver acesso à Internet, execute o seguinte comando:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

Você também pode baixar o módulo em outro computador que tenha acesso à Internet e copiar o módulo resultante para `C:\Program Files\WindowsPowerShell\Modules` o no host protegido.

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

Depois que o módulo estiver instalado, você estará pronto para provisionar sua VM blindada.

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

Se o arquivo de resposta de dados de blindagem incluir valores de especialização, você poderá fornecer os valores de substituição para New-ShieldedVM. Neste exemplo, o arquivo de resposta é configurado com valores de espaço reservado para um endereço IPv4 estático.

```powershell
$specializationValues = @{
    "@IP4Addr-1@" = "192.168.1.10/24"
    "@MacAddr-1@" = "Ethernet"
    "@Prefix-1-1@" = "24"
    "@NextHop-1-1@" = "192.168.1.254"
}
New-ShieldedVM -Name 'MyStaticIPVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -SpecializationValues $specializationValues -Wait

```

Se o seu disco de modelo contiver um sistema operacional baseado em Linux, inclua o `-Linux` sinalizador ao executar o comando:

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

Verifique o conteúdo da ajuda usando `Get-Help New-ShieldedVM -Full` para saber mais sobre outras opções que você pode passar para o cmdlet.

Depois que a VM concluir o provisionamento, ela entrará na fase de especialização específica do sistema operacional, após a qual estará pronta para uso.
Certifique-se de conectar a VM a uma rede válida para que você possa se conectar a ela quando ela estiver em execução (usando RDP, PowerShell, SSH ou sua ferramenta de gerenciamento preferencial).

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>Executando VMs blindadas em um cluster Hyper-V

Se você estiver tentando implantar VMs blindadas em hosts protegidos em cluster (usando um cluster de failover do Windows), poderá configurar a VM blindada para que ela seja altamente disponível usando o seguinte cmdlet:

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

A VM blindada agora pode ser migrada ao vivo no cluster.

## <a name="next-step"></a>Próxima etapa

> [!div class="nextstepaction"]
> [Implantar um blindado usando o VMM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)