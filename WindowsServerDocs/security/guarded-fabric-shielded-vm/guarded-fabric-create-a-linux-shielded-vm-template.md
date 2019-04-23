---
title: Criar do Linux blindada disco de modelo VM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: e791d18e027e5e3e1c5b9c52659641ff588a96a2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59858667"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>Criar do Linux blindada disco de modelo VM

> Aplica-se a: Windows Server do Windows Server (canal semestral), de 2019 

Este tópico explica como preparar um disco de modelo para máquinas virtuais que podem ser usados para criar uma instância de locatário de um ou mais VMs do Linux blindada.

## <a name="prerequisites"></a>Pré-requisitos

Para preparar e testar a VM do Linux blindada, será necessário os seguintes recursos disponíveis:

- Um servidor com capababilities de virtualização que executa o Windows Server, versão 1709 ou posterior
- Um segundo computador (Windows 10 ou Windows Server 2016) capaz de executar o Gerenciador do Hyper-V para se conectar ao console da VM em execução
- Uma imagem ISO para uma do Linux com suporte blindado sistemas operacionais de VM:
    - Ubuntu 16.04 LTS com kernel 4.4
    - Red Hat Enterprise Linux 7.3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- Acesso à Internet para baixar o pacote lsvmtools e atualizações do sistema operacional

> [!IMPORTANT]
> Versões mais recentes do anterior que OSEs Linux podem incluir um bug conhecido de driver TPM que impedirá de provisionamento com êxito como VMs blindadas.
> Não é recomendável que você atualize seus modelos ou VMs blindadas para uma versão mais recente até que uma correção está disponível.
> A lista de sistemas operacionais com suporte acima será atualizada quando as atualizações são tornadas públicas.

## <a name="prepare-a-linux-vm"></a>Preparar uma VM do Linux

VMs blindadas são criadas de discos de modelo seguro.
Discos de modelo contém o sistema operacional para a VM e os metadados, incluindo uma assinatura digital de partições /boot e /root garantir que componentes principais do sistema operacional não são modificados antes da implantação.

Para criar um disco de modelo, crie primeiro uma VM (não blindada) comum que você prepare como a imagem base para futuros VMs blindadas.
O software instalado e as alterações de configuração feitas a essa VM serão aplicada a todas as VMs blindadas criadas a partir desse disco de modelo.
Essas etapas orientarão requisitos mínimos para preparar uma VM do Linux para templatization.

> [!NOTE]
> Criptografia de disco Linux é configurada quando o disco é particionado.
> Isso significa que você deve criar uma nova VM previamente criptografada usando dm-crypt para criar do Linux blindadas disco de modelo VM.


1.  No servidor de virtualização, certifique-se de que o Hyper-V e os recursos de suporte do Hyper-V de guardião de Host são instalados, executando os seguintes comandos em um console do PowerShell com privilégios elevados:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  Baixe a imagem ISO de uma fonte confiável e armazená-lo em seu servidor de virtualização ou em um compartilhamento de arquivos acessível ao seu servidor de virtualização.

3.  Em seu computador de gerenciamento executando o Windows Server versão 1709, instale blindado VM administração ferramentas de servidor remoto executando o seguinte comando:

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  Abra **Gerenciador do Hyper-V** em seu computador de gerenciamento e se conectar ao seu servidor de virtualização.
    Você pode fazer isso clicando em "Conectar-se ao servidor..." no painel de ações ou o botão direito do mouse clicando em Gerenciador do Hyper-V e escolhendo "conectem ao servidor..." Forneça o nome DNS para seu servidor Hyper-V e, se necessário, as credenciais necessárias para se conectar a ele.

5.  Usando o Gerenciador do Hyper-V [configurar um comutador externo](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) no seu servidor de virtualização para que a VM do Linux possam acessar a Internet para obter atualizações.

6.  Em seguida, crie uma nova máquina virtual para instalar o sistema operacional Linux em.
    No painel de ações, clique em **New** > **Máquina Virtual** para abrir o assistente.
    Forneça um nome amigável para sua VM, como "Linux Pre-templatized" e clique em **próxima**.

7.  Na segunda página do assistente, selecione **geração 2** para garantir que a VM é provisionada com um perfil de firmware com base em UEFI.

8.  Conclua o restante do Assistente de acordo com suas preferências.
    Não use um disco diferencial para essa VM; discos de modelo VM blindados não é possível usar discos diferenciais.
    Por fim, conecte-se a imagem ISO que você baixou anteriormente para a unidade de DVD virtual para esta VM para que você possa instalar o sistema operacional.

9.  No Gerenciador do Hyper-V, selecione sua VM recém-criada e clique em **conectar...**  no painel de ações para anexar a um console do virtual da VM.
    Na janela que aparece, clique em **iniciar** para ativar a máquina virtual.

10. Continue com o processo de instalação para a sua distribuição Linux selecionada.
    Embora cada distribuição Linux usa um Assistente de instalação diferente, os seguintes requisitos devem ser atendidos para VMs que se tornarão Linux blindadas de discos de modelo VM:

    - O disco deve ser particionado usando o layout de tabela de particionamento de GUID (GPT)
    - A partição raiz deve ser criptografada com dm-crypt. A frase secreta deve ser definida como **frase secreta** (todas as letras minúsculas). Essa frase secreta será aleatória e a partição criptografada novamente quando uma máquina virtual blindada é provisionada.
    - A partição de inicialização deve usar o **ext2** sistema de arquivos

11. Depois do sistema operacional Linux foi totalmente inicializado e ter entrado, é recomendável que você instale o kernel linux virtuais e pacotes de serviços de integração do Hyper-V associados.
    Além disso, você desejará instalar um servidor SSH ou outra ferramenta de gerenciamento remoto para acessar a VM depois que ela é blindada.

    No Ubuntu, execute o seguinte comando para instalar esses componentes:

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    No RHEL, execute o seguinte comando:

    ```bash
    sudo yum install hyperv-daemons openssh-server
    sudo service sshd start
    ```

    E, em SLES, execute o seguinte comando:

    ```bash
    sudo zypper install hyper-v
    sudo chkconfig hv_kvp_daemon on
    sudo systemctl enable sshd
    ```

12. Configure seu sistema operacional Linux conforme desejado.
    Qualquer software instalar, contas de usuário que você adiciona, e alterações feitas na configuração em todo o sistema aplicará a todas as VMs futuras criadas a partir desse disco de modelo.
    Você deve evitar salvar qualquer segredos ou pacotes desnecessários no disco.

13. Se você estiver planejando usar o System Center Virtual Machine Manager para implantar suas VMs, instale o agente convidado do VMM para habilitar o VMM especializar o seu sistema operacional durante o provisionamento da VM.
    A especialização permite que cada VM para ser configurado com segurança com diferentes usuários e chaves SSH, as configurações da rede e as etapas de configuração personalizado.
    Saiba como [obter e instalar o agente convidado do VMM](https://docs.microsoft.com/system-center/vmm/vm-linux#install-the-vmm-guest-agent) na documentação do VMM.

14. Em seguida, [adicionar o repositório de Software do Microsoft Linux para o Gerenciador de pacotes](../../administration/linux-package-repository-for-microsoft-software.md).

15. Usando o Gerenciador de pacotes, instalar o pacote de lsvmtools que contém o Linux blindadas shim do carregador de inicialização VM, o provisionamento de componentes e a ferramenta de preparação do disco.

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. Quando você terminar Personalizando o sistema operacional Linux, localize o programa de instalação lsvmprep em seu sistema e executá-lo.
    
    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. Desligue a VM.

16. Se você tirou quaisquer pontos de verificação de sua VM (incluindo pontos de verificação automáticos criados pelo Hyper-V com o Windows 10 Fall Creators Update), certifique-se de excluí-los antes de continuar.
    Pontos de verificação criam discos diferenciais (. avhdx) que não são suportados pelo Assistente de disco de modelo.
    
    Para excluir os pontos de verificação, abra **Gerenciador do Hyper-V**, selecione sua VM, clique com botão direito do ponto de verificação de nível mais alto no painel de pontos de verificação e clique em **excluir subárvore de ponto de verificação**.

    ![Excluir todos os pontos de verificação para o modelo de VM no Gerenciador do Hyper-V](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>Proteger o disco de modelo

A VM que você preparou na seção anterior está quase pronta para ser usado como do Linux blindada disco de modelo VM.
A última etapa é executar o disco pelo Assistente de disco de modelo, que serão de hash e assinar digitalmente o estado atual das partições raiz e de inicialização.
O hash e assinatura digital são verificadas quando uma máquina virtual blindada é provisionada para garantir que não há alterações não autorizadas feitas às duas partições entre a criação do modelo e a implantação.

### <a name="obtain-a-certificate-to-sign-the-disk"></a>Obter um certificado para assinar o disco

Para assinar digitalmente as medidas do disco, você precisará obter um certificado no computador onde você executará o Assistente de disco de modelo.
O certificado deve atender aos seguintes requisitos:

Propriedade do certificado | Valor obrigatório
---------------------|---------------
Algoritmo de chave | RSA
Tamanho mínimo da chave | 2048 bits
Algoritmo de assinatura | SHA256 (recomendado)
Uso de chave | Assinatura digital

Detalhes sobre o certificado serão exibidos para locatários quando criarem seus arquivos de dados de blindagem e autorizar o discos que confiam.
Portanto, é importante obter este certificado de autoridade de certificação mutuamente confiável para você e seus locatários.
Cenários de negócios em que você está o hoster e o locatário, você pode considerar emitir esse certificado da autoridade de certificação empresarial.
Proteger esse certificado com cuidado, pois alguém em posse do certificado pode criar novos discos de modelo que são confiáveis o mesmo que o disco autêntico.

Em um ambiente de laboratório de teste, você pode criar um certificado autoassinado com o seguinte comando do PowerShell:

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>O disco com o cmdlet do Assistente de disco de modelo de processo

Copie o disco de modelo e o certificado para um computador executando o Windows Server, versão 1709, em seguida, execute os seguintes comandos para iniciar o processo de assinatura.
O VHDX que você fornecer para o `-Path` parâmetro será será substituída com o disco de modelo atualizado, portanto, certifique-se de fazer uma cópia antes de executar o comando.

> [!IMPORTANT]
> As ferramentas de administração de servidor remoto disponível no Windows Server 2016 ou Windows 10 não pode ser usadas para preparar do Linux blindada disco de modelo VM.
> Usar somente o [TemplateDisk proteger](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) cmdlet disponível no Windows Server, versão 1709 ou as ferramentas de administração de servidor remoto disponível no Windows Server 2019 para preparar do Linux blindadas disco de modelo VM.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

O disco de modelo agora está pronto para ser usado para provisionar VMs blindadas do Linux.
Se você estiver usando o System Center Virtual Machine Manager para implantar a VM, agora você pode copiar o VHDX para a biblioteca do VMM.

Talvez você queira extrair o catálogo de assinatura de volume de VHDX.
Esse arquivo é usado para fornecer informações sobre o certificado de autenticação, o nome do disco e a versão para os proprietários da VM que desejam usar seu modelo.
Eles precisam importar esse arquivo para o Assistente de arquivo de dados de blindagem para autorizá-lo, o autor do modelo em posse do certificado de autenticação, para criar este e discos de modelo futuras para eles.

Para extrair o catálogo de assinatura de volume, execute o seguinte comando no PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
