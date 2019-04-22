---
redirect_url: guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md
title: Blindado de VMs blindadas para locatários – criando uma nova VM no local e movê-lo para uma malha protegida
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0ca1efa0-01f9-4b6f-87d4-c66db00d7d70
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 2da1e33d24fa6d68815f4fbc0891be0616004856
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817467"
---
# <a name="shielded-vms-for-tenants---creating-a-new-shielded-vm-on-premises-and-moving-it-to-a-guarded-fabric"></a>Blindado de VMs blindadas para locatários – criando uma nova VM no local e movê-lo para uma malha protegida

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016


<!-- NOTE THAT THIS FILE HAS A "redirect_url" LINE IN THE METADATA. EVENTUALLY WE WILL PROBABLY STRIP OUT THE DETAILED METADATA AND THE CONTENT BELOW, SO IT'S PURELY A REDIRECTED TOPIC. However, as of mid-November 2016, we're still deciding. -->



Este tópico descreve as etapas para criar uma VM blindada usando somente Hyper-V; ou seja, sem Virtual Machine Manager, discos de modelo ou um arquivo de dados de blindagem. Isso é um cenário incomum para a nuvem pública mais ambientes de hospedagem, mas pode ser útil ao testar uma malha protegida ou no enterprise cenários em que uma máquina virtual está sendo movida de uma malha departamental para compartilhado de infraestrutura de TI em devem ser criptografados antes da migração.

Para entender como este tópico se encaixa no processo geral de implantação de VMs blindadas, consulte [hospedar as etapas de configuração de provedor de serviço para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="import-the-guardian-configuration-on-the-tenant-hyper-v-server"></a>Importar a configuração no servidor de locatário do Hyper-V do guardião

1.  Antes de começar o procedimento, verifique se que você está em um host Hyper-V executando o Windows Server 2016 com as seguintes funções e recursos instalados:

    - Função

        - Hyper-V

    - Recursos

        - Ferramentas de administração de servidor remoto\\as ferramentas de administração de recursos\\ferramentas de VM Blindada

    > [!NOTE]
    > O host usado aqui deverá *não* ser um host na malha protegida. Isso é um host separado, onde as VMs são preparadas antes de ser movido para a malha protegida.

2.  Antes de executar uma VM blindada neste computador, será necessário garantir a que segurança baseada em virtualização é habilitado e em execução. Execute o seguinte comando em uma janela elevada do Windows PowerShell para instalar o recurso de suporte de Hyper-V de guardião de Host. Isso irá configurar todas as configurações necessárias em seu computador para poder executar VMs blindadas.

        Install-WindowsFeature HostGuardian

3.  Você precisará adquirir os metadados do guardião para a malha protegida em que a VM será executado. Esses metadados são usados para autorizar que a malha a execute sua máquina virtual blindada. Como obter essas informações serão diferente para cada provedor de serviços de hospedagem ou enterprise. O hoster (ou, se você tiver acesso à rede de malha protegida) pode obter essas informações executando o seguinte comando do Windows PowerShell:

        Invoke-WebRequest 'http://hgs.bastion.local/KeyProtection/service/metadata/2014-07/metadata.xml' -OutFile .\RelecloudGuardian.xml

    No exemplo acima, "hgs" é o nome de rede distribuída do cluster HGS e "bastion.local" é o nome do domínio HGS.

4.  Para importar a chave de guardião, que será necessário em um procedimento posterior, execute o comando a seguir.

    Para &lt;caminho&gt;&lt;Filename&gt;, substitua o caminho e nome de arquivo do XML do arquivo é salvo na etapa anterior, por exemplo: **C:\\temp\\GuardianKey.xml**

    Para &lt;GuardianName&gt;, especifique um nome para seu provedor de hospedagem ou data center corporativo, por exemplo, **HostingProvider1**. Registre o nome para o próximo procedimento.

    Incluir **- AllowUntrustedRoot** somente se o servidor HGS foi configurado com certificados autoassinados. (Esses certificados são parte do serviço de proteção de chave em HGS.)

        Import-HgsGuardian -Path '<Path><Filename>' -Name '<GuardianName>' -AllowUntrustedRoot

## <a name="create-a-new-shielded-virtual-machine-on-the-host"></a>Criar uma nova máquina virtual blindada no host

Neste procedimento, você irá criar uma máquina virtual no host Hyper-V e prepará-lo para exportação para o seu provedor de hospedagem ou o administrador do data center, que pode ser executado em um host protegido.

Como parte do procedimento, você criará um protetor de chave que contém dois elementos importantes:

-   **Proprietário**: No protetor de chave, você - ou, mais provavelmente, o grupo de funcionar, que compartilha elementos, como certificados de segurança - é identificado como "proprietário" da VM. Sua identidade como proprietário é representada por um certificado que, se você executar os comandos, conforme mostrado, é gerado como um certificado autoassinado. Opcionalmente, você pode em vez disso, use um certificado com suporte pela infraestrutura PKI e omitir as **- AllowUntrustedRoot** parâmetro nos comandos.

-   **Guardiões**: Também no protetor de chave, seu provedor de hospedagem ou data center corporativo (que executa o HGS e hosts protegidos) é identificado como "guardião." O guardião é representado pela chave de guardião importada no procedimento anterior, [importar a configuração no servidor de locatário do Hyper-V do guardião](#import-the-guardian-configuration-on-the-tenant-hyper-v-server).

Para obter uma ilustração que mostra o protetor de chave, que é um elemento em um arquivo de dados de blindagem, consulte [o que é de dados de blindagem e por que é necessário?](guarded-fabric-and-shielded-vms.md#what-is-shielding-data-and-why-is-it-necessary).

1.  Em um locatário do host do Hyper-V, para criar uma máquina virtual de geração 2, execute o comando a seguir.

    Para &lt;ShieldedVMname&gt;, especifique um nome para a VM, por exemplo: **ShieldVM1**
    
    Para &lt;VHDPath&gt;, especifique um local para armazenar o VHDX da VM, por exemplo: **C:\\VMs\\ShieldVM1\\ShieldVM1.vhdx**
    
    Para &lt;nnGB&gt;, especifique um tamanho para VHDX, por exemplo: **60GB**

        New-VM -Generation 2 -Name "<ShieldedVMname>" -NewVHDPath <VHDPath>.vhdx -NewVHDSizeBytes <nnGB>

2.  Instalar um sistema operacional compatível (cliente do Windows Server 2012 ou posterior, Windows 8 ou superior) na VM e habilitar a conexão de área de trabalho remota e a regra de firewall correspondente. Registrar o endereço IP da VM e/ou nome DNS; Você precisará dele para se conectar remotamente a ela.

3.  Use o RDP para se conectar à VM remotamente e verificar se o RDP e o firewall estão configuradas corretamente. Como parte do processo de blindagem, acesso ao console para a máquina virtual por meio do Hyper-V será desativado, portanto, é importante garantir que você possa gerenciar remotamente o sistema pela rede.

4.  Para criar um novo protetor de chave (descrito no início desta seção), execute o comando a seguir.

    Para &lt;GuardianName&gt;, use o nome especificado no procedimento anterior, por exemplo: **HostingProvider1**

    Incluir **AllowUntrustedRoot -** para permitir que os certificados autoassinados.

        $Guardian = Get-HgsGuardian -Name '<GuardianName>'

        $Owner = New-HgsGuardian -Name 'Owner' -GenerateCertificates

        $KP = New-HgsKeyProtector -Owner $Owner -Guardian $Guardian -AllowUntrustedRoot

    Se você quiser para mais de um datacenter poderá executar sua VM blindada (por exemplo, um site de recuperação de desastres e um provedor de nuvem pública), você pode fornecer uma lista dos guardiões para o **-guardião** parâmetro. Para obter mais informações, consulte [New-HgsKeyProtector] (https://docs.microsoft.com/powershell/module/hgsclient/new-hgskeyprotector?view=win10-ps.

5.  Para habilitar o vTPM usando o protetor de chave, execute o comando a seguir. Para &lt;ShieldedVMname&gt;, use o mesmo nome VM usado nas etapas anteriores.

        $VMName="<ShieldedVMname>"

        Stop-VM -Name $VMName -Force

        Set-VMKeyProtector -VMName $VMName -KeyProtector $KP.RawData

        Set-VMSecurityPolicy -VMName $VMName -Shielded $true

        Enable-VMTPM -VMName $VMName

6.  Para iniciar a VM para verificar se o protetor de chave está funcionando com certificados de proprietário local, execute o comando a seguir.

        Start-VM -Name $VMName

7.  Verifique se que a VM foi iniciada no console do Hyper-V.

8.  Use o RDP para se conectar à VM remotamente e habilitar o BitLocker em todas as partições de todos os VHDXs que estão anexados à VM blindada.

    > [!IMPORTANT]
    > Antes de prosseguir para a próxima etapa, aguarde para conclusão em todas as partições em que você habilitou a criptografia BitLocker.

9.  Desligar a VM quando você estiver pronto para movê-lo para a malha protegida.

10.  No servidor de locatário do Hyper-V, exporte a máquina virtual usando a ferramenta de sua escolha (Windows PowerShell ou Gerenciador do Hyper-V). Em seguida, organize para os arquivos a serem copiados para um host protegido mantido por seu provedor de hospedagem ou data center corporativo.

11.  **Para o datacenter corporativo ou de provedor de hospedagem**:

    Importe a VM blindada usando o Gerenciador do Hyper-V ou o Windows PowerShell. Você deve importar o arquivo de configuração de VM do proprietário da VM para iniciar a máquina virtual. Isso ocorre porque o protetor de chave e o TPM virtual da VM são armazenados no arquivo de configuração. Se a VM é configurada para executar em malha protegida, ele deve ser capaz de ser iniciado com êxito.

## <a name="see-also"></a>Consulte também

- [Etapas de configuração de provedor de serviço de hospedagem de hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
