---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: VMs blindadas para locatários-criando uma nova VM blindada localmente e movendo-a para uma malha protegida
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: a4b5ff2942c8485a4c10770a4374d56734f7f3c9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402387"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>VMs blindadas para locatários-criando uma nova VM blindada localmente e movendo-a para uma malha protegida

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

Este tópico descreve as etapas para criar uma VM blindada usando apenas o Hyper-V; ou seja, sem Virtual Machine Manager, discos de modelo ou um arquivo de dados de blindagem. Esse é um cenário incomum para a maioria dos ambientes de Hospedagem de nuvem pública, mas pode ser útil ao testar uma malha protegida ou em cenários empresariais em que uma VM está sendo movida de uma malha departamental para uma infraestrutura de ti compartilhada e deve ser criptografada antes da migração.

Para entender como este tópico se encaixa no processo geral de implantação de VMs blindadas, consulte [as etapas de configuração do provedor de serviço de hospedagem para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>Importar a configuração do guardião no servidor Hyper-V do locatário

1.  Antes de iniciar o procedimento, verifique se você está em um host Hyper-V executando o Windows Server 2016 com as seguintes funções e recursos instalados:

    - Role

        - Hyper-V

    - Recursos

        - Ferramentas de Administração de Servidor Remoto @ no__t-0Feature ferramentas de administração @ no__t-1Shielded ferramentas de VM

    > [!NOTE]
    > O host usado aqui *não* deve ser um host na malha protegida. Esse é um host separado em que as VMs são preparadas antes de serem movidas para a malha protegida.

2.  Para poder executar uma VM blindada neste computador, você precisará garantir que a segurança baseada em virtualização esteja habilitada e em execução. Execute o seguinte comando em uma janela do Windows PowerShell com privilégios elevados para instalar o recurso de suporte do Hyper-V do guardião de host. Isso definirá todas as configurações necessárias em seu computador para poder executar VMs blindadas.

        Install-WindowsFeature HostGuardian

3.  Você precisará adquirir os metadados do guardião para a malha protegida em que sua VM será executada. Esses metadados são usados para autorizar essa malha a executar a VM blindada. A forma como você obtém essas informações será diferente para cada provedor de serviços de hospedagem ou empresa. O hoster (ou você, se você tiver acesso à rede de malha protegida) poderá adquirir essas informações executando o seguinte comando do Windows PowerShell:

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    No exemplo acima, "HgS" é o nome de rede distribuída do cluster HGS e "bastiões. local" é o nome do domínio HGS.

4.  Para importar a chave do guardião, que será necessária em um procedimento posterior, execute o comando a seguir.

    Para &lt;Path @ no__t-1 @ no__t-2Filename @ no__t-3, substitua o caminho e o nome do arquivo XML que você salvou na etapa anterior, por exemplo: **C: @no__t -1temp\\GuardianKey.xml**

    Para &lt;GuardianName @ no__t-1, especifique um nome para seu provedor de hospedagem ou Datacenter corporativo, por exemplo, **HostingProvider1**. Registre o nome para o próximo procedimento.

    Include **-AllowUntrustedRoot** somente se o servidor HgS tiver sido configurado com certificados autoassinados. (Esses certificados fazem parte do serviço de proteção de chave no HGS.)

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>Criar uma nova máquina virtual blindada no host

Neste procedimento, você criará uma máquina virtual no host Hyper-V e a preparará para exportação para o seu provedor de hospedagem ou administrador de datacenter, que poderá executá-lo em um host protegido.

Como parte do procedimento, você criará um protetor de chave que contém dois elementos importantes:

-   **Proprietário**: No protetor de chave, você-ou mais provavelmente, o grupo no qual você trabalha, que compartilha elementos de segurança como certificados – são identificados como "proprietário" da VM. Sua identidade como proprietário é representada por um certificado que, se você executar os comandos, conforme mostrado, será gerado como um certificado autoassinado. Opcionalmente, você pode usar um certificado apoiado pela infraestrutura PKI e omitir o parâmetro **-AllowUntrustedRoot** nos comandos.

-   **Guardiões**: Também no protetor de chave, seu provedor de hospedagem ou Datacenter corporativo (que executa HGS e hosts protegidos) é identificado como "guardião". O Guardião é representado pela chave do Guardião que você importou no procedimento anterior, [importe a configuração do guardião no servidor do Hyper-V do locatário](#import-the-guardian-configuration-on-the-tenant-hyper-v-server).

Para uma ilustração que mostra o protetor de chave, que é um elemento em um arquivo de dados de blindagem, consulte [o que são os dados de blindagem e por que ele é necessário?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

1. Em um host Hyper-V de locatário, para criar uma nova máquina virtual de geração 2, execute o comando a seguir.

   Para &lt;ShieldedVMname @ no__t-1, especifique um nome para a VM, por exemplo: **ShieldVM1**
    
   Para &lt;VHDPath @ no__t-1, especifique um local para armazenar o VHDX da VM, por exemplo: **C: \\VMs @ no__t-2ShieldVM1\\ShieldVM1.vhdx**
    
   Para &lt;nnGB @ no__t-1, especifique um tamanho para o VHDX, por exemplo: **60 GB**

       New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2. Instale um sistema operacional com suporte (Windows Server 2012 ou superior, cliente do Windows 8 ou superior) na VM e habilite a conexão de área de trabalho remota e a regra de firewall correspondente. Registre o endereço IP e/ou o nome DNS da VM; Você precisará dele para se conectar remotamente a ele.

3. Use o RDP para se conectar remotamente à VM e verificar se o RDP e o firewall estão configurados corretamente. Como parte do processo de blindagem, o acesso ao console para a máquina virtual por meio do Hyper-V será desabilitado, portanto, é importante garantir que você possa gerenciar remotamente o sistema pela rede.

4. Para criar um novo protetor de chave (descrito no início desta seção), execute o comando a seguir.

   Para &lt;GuardianName @ no__t-1, use o nome especificado no procedimento anterior, por exemplo: **HostingProvider1**

   Include **-AllowUntrustedRoot** para permitir certificados autoassinados.

       $Guardian = Get-HgsGuardian -Name '<GuardianName>'

       $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

       $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

   Se desejar que mais de um datacenter seja capaz de executar sua VM blindada (por exemplo, um site de recuperação de desastre e um provedor de nuvem pública), você poderá fornecer uma lista de guardiões para o parâmetro **-guardião** . Para obter mais informações, consulte [New-HgsKeyProtector] (https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps.

5. Para habilitar o vTPM usando o protetor de chave, execute o comando a seguir. Para &lt;ShieldedVMname @ no__t-1, use o mesmo nome de VM usado nas etapas anteriores.

       $VMName="<ShieldedVMname>"

       Stop-VM -Name $VMName -Force

       Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

       Set-VMSecurityPolicy -VMName $VMName -Shielded $true

       Enable-VMTPM -VMName $VMName

6. Para iniciar a VM para verificar se o protetor de chave está funcionando com certificados de proprietário local, execute o comando a seguir.

       Start-VM -Name $VMName

7. Verifique se a VM foi iniciada no console do Hyper-V.

8. Use o RDP para se conectar remotamente à VM e habilitar o BitLocker em todas as partições em todos os VHDXs anexados à VM blindada.

   > [!IMPORTANT]
   > Antes de prosseguir para a próxima etapa, aguarde a conclusão da criptografia do BitLocker em todas as partições em que você a habilitou.

9. Desligue a VM quando estiver pronto para movê-la para a malha protegida.

10. No servidor Hyper-V do locatário, exporte a VM usando a ferramenta de sua escolha (Windows PowerShell ou Gerenciador do Hyper-V). Em seguida, organize os arquivos a serem copiados para um host protegido mantido pelo seu provedor de hospedagem ou Datacenter corporativo.

11. **Para o provedor de hospedagem ou Datacenter corporativo**:

    Importe a VM blindada usando o Gerenciador do Hyper-V ou o Windows PowerShell. Você deve importar o arquivo de configuração da VM do proprietário da VM para iniciar a VM. Isso ocorre porque o protetor de chave e o TPM virtual da VM estão armazenados no arquivo de configuração. Se a VM estiver configurada para ser executada na malha protegida, ela deverá ser capaz de iniciar com êxito.

## <a name="see-also"></a>Consulte também

- [Etapas de configuração do provedor de serviços de hospedagem para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
