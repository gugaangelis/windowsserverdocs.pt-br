---
title: Criar uma VM blindada usando o PowerShell
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 76be26e107bd16165367d5432e1dd757dea2f9b4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855407"
---
# <a name="create-a-shielded-vm-using-powershell"></a>Criar uma VM blindada usando o PowerShell

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Em produção, você normalmente usaria um Gerenciador de malha (por exemplo, o VMM) para implantar VMs blindadas. No entanto, as etapas ilustradas abaixo permitem que você implante e validar todo o cenário sem um Gerenciador de malha.

Em resumo, você criará um disco de modelo, um arquivo de dados de blindagem, um arquivo de resposta de instalação autônoma e outros artefatos de segurança em qualquer computador, em seguida, copie esses arquivos para um host protegido e provisionar a VM blindada.

## <a name="create-a-signed-template-disk"></a>Criar um disco de modelo assinado

Para criar uma nova VM blindada, você primeiro precisa de um disco de modelo VM blindado é previamente criptografado com seu sistema operacional volume (ou partições de inicialização e raiz no Linux) assinado.
Siga os links abaixo para obter mais informações sobre como criar um disco de modelo.

- [Preparar um disco de modelo do Windows](guarded-fabric-create-a-shielded-vm-template.md)
- [Preparar um disco de modelo do Linux](guarded-fabric-create-a-linux-shielded-vm-template.md)

Você também precisará de uma cópia do catálogo de assinatura de volume do disco para criar o arquivo de dados de blindagem.
Para salvar esse arquivo, execute o seguinte comando no computador em que você criou o disco de modelo:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath "C:\temp\MyTemplateDisk.vhdx" -VolumeSignatureCatalogPath "C:\temp\MyTemplateDiskCatalog.vsc"
```

## <a name="download-guardian-metadata"></a>Baixar guardião metadados

Para cada uma das malhas de virtualização no qual você deseja executar sua VM blindada, você precisará obter os metadados do guardião para clusters HGS dos malhas.
Seu provedor de hospedagem deve ser capaz de fornecer essas informações para você.

Se você estiver em um ambiente corporativo e pode se comunicar com o servidor HGS, os metadados do guardião estão disponível em *http://\<HGSCLUSTERNAME\>/KeyProtection/service/metadata/2014-07/metadata.xml*

## <a name="create-shielding-data-pdk-file"></a>Criar arquivo de dados de blindagem (PDK)

Os dados de blindagem é criado e pertencem a proprietários da VM de locatário e contêm segredos necessários para criar VMs blindadas que devem ser protegidas contra o administrador da malha, como uma senha de administrador da VM blindada.
Os dados de blindagem é criptografado, de modo que somente os servidores HGS e o locatário podem descriptografá-la.
Uma vez criado pelo proprietário do locatário/VM, o arquivo PDK resultante deve ser copiado para a malha protegida.
Para obter mais informações, consulte [o que é de dados de blindagem e por que é necessário?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

Além disso, será necessário um arquivo de resposta de instalação autônoma (Unattend. XML para Windows, que varia para Linux). Ver [criar um arquivo de resposta](guarded-fabric-tenant-creates-shielding-data.md#create-an-answer-file) para obter orientação sobre o que incluir no arquivo de resposta.

Execute os seguintes cmdlets em um computador com as ferramentas de administração de servidor remoto para VMs Blindadas instalado.
Se você estiver criando um PDK para uma VM do Linux, você deve fazer isso em um servidor executando o Windows Server, versão 1709 ou posterior.

 
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
    
## <a name="provision-shielded-vm-on-a-guarded-host"></a>Provisionar blindadas VM em um host protegido
Copie o arquivo de disco de modelo (ServerOS.vhdx) e o arquivo PDK (contoso.pdk) para o host protegido para se preparar para implantação.

O host protegido, instale o módulo de PowerShell de ferramentas de malha protegida, que contém o cmdlet New-ShieldedVM para simplificar o processo de provisionamento. Se o host protegido tem acesso à Internet, execute o seguinte comando:

```powershell
Install-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0
```

Você também pode baixar o módulo em outro computador que tem a Internet acessar e copiar o módulo resultante `C:\Program Files\WindowsPowerShell\Modules` o host protegido.

```powershell
Save-Module GuardedFabricTools -Repository PSGallery -MinimumVersion 1.0.0 -Path C:\temp\
```

Depois que o módulo é instalado, você está pronto para provisionar sua VM blindada.

```powershell
New-ShieldedVM -Name 'MyShieldedVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait
```

Se o disco de modelo contém um sistema operacional baseado em Linux, inclua o `-Linux` sinalizar ao executar o comando:

```powershell
New-ShieldedVM -Name 'MyLinuxVM' -TemplateDiskPath 'C:\temp\MyTemplateDisk.vhdx' -ShieldingDataFilePath 'C:\temp\Contoso.pdk' -Wait -Linux
```

Verificar o conteúdo de ajuda usando `Get-Help New-ShieldedVM -Full` para saber mais sobre outras opções que você pode passar para o cmdlet.

Depois que a VM concluir o provisionamento, ele irá inserir a fase de especialização específicas do sistema operacional, após o qual ele estará pronto para uso.
Certifique-se de conectar-se a VM para uma rede válida para que você pode se conectar a ele quando ele estiver em execução (usando sua ferramenta de gerenciamento preferenciais, PowerShell, SSH ou RDP).

## <a name="running-shielded-vms-on-a-hyper-v-cluster"></a>Executar VMs Blindadas em um cluster do Hyper-V

Se você estiver tentando implantar VMs blindadas em hosts clusterizados de protegida (usando um Cluster de Failover do Windows), você pode configurar a VM blindada para ser altamente disponível usando o seguinte cmdlet:

```powershell
Add-ClusterVirtualMachineRole -VMName 'MyShieldedVM' -Cluster <Hyper-V cluster name>
```

A máquina virtual blindada agora pode ser ao vivo migrados dentro do cluster.

## <a name="next-step"></a>Próximas etapas

>[!div class="nextstepaction"]
[Implantar um blindada usando o VMM](guarded-fabric-tenant-deploys-shielded-vm-using-vmm.md)