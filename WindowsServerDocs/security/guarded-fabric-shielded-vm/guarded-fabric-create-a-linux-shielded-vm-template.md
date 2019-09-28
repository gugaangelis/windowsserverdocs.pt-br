---
title: Criar um disco de modelo de VM blindada Linux
ms.custom: na
ms.prod: windows-server
ms.topic: article
ms.assetid: d0e1d4fb-97fc-4389-9421-c869ba532944
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 66d5f70f747a6209f2856afde58b6f486ea597f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71386711"
---
# <a name="create-a-linux-shielded-vm-template-disk"></a>Criar um disco de modelo de VM blindada Linux

> Aplica-se a: Windows Server 2019, Windows Server (canal semestral), 

Este tópico explica como preparar um disco de modelo para VMs blindadas Linux que podem ser usadas para criar uma instância de uma ou mais VMs de locatário.

## <a name="prerequisites"></a>Pré-requisitos

Para preparar e testar uma VM blindada Linux, você precisará dos seguintes recursos disponíveis:

- Um servidor com virtualização capababilities executando o Windows Server, versão 1709 ou posterior
- Um segundo computador (Windows 10 ou Windows Server 2016) capaz de executar o Gerenciador do Hyper-V para se conectar ao console da VM em execução
- Uma imagem ISO para um dos SOS de VM blindadas Linux com suporte:
    - Ubuntu 16, 4 LTS com o kernel 4,4
    - Red Hat Enterprise Linux 7.3
    - SUSE Linux Enterprise Server 12 Service Pack 2
- Acesso à Internet para baixar o pacote lsvmtools e as atualizações do sistema operacional

> [!IMPORTANT]
> As versões mais recentes dos SOS Linux anteriores podem incluir um bug de driver TPM conhecido que impedirá que eles se provisionem com êxito como VMs blindadas.
> Não é recomendável que você atualize seus modelos ou VMs blindadas para uma versão mais recente até que uma correção esteja disponível.
> A lista de sistemas operacionais com suporte acima será atualizada quando as atualizações forem feitas públicas.

## <a name="prepare-a-linux-vm"></a>Preparar uma VM do Linux

As VMs blindadas são criadas a partir de discos de modelo seguros.
Os discos de modelo contêm o sistema operacional para a VM e os metadados, incluindo uma assinatura digital das partições/boot e/root, para garantir que OS principais componentes do sistema operacional não sejam modificados antes da implantação.

Para criar um disco de modelo, você deve primeiro criar uma VM regular (não blindada) que será preparada como a imagem base para VMs blindadas futuras.
O software que você instala e as alterações de configuração feitas nessa VM serão aplicados a todas as VMs blindadas criadas a partir desse disco de modelo.
Essas etapas guiarão você pelos requisitos mínimos para obter uma VM Linux pronta para Templatization.

> [!NOTE]
> A criptografia de disco do Linux é configurada quando o disco é particionado.
> Isso significa que você deve criar uma nova VM que é previamente criptografada usando DM-cript para criar um disco de modelo de VM blindado do Linux.


1.  No servidor de virtualização, verifique se o Hyper-V e os recursos de suporte do Hyper-V do guardião de host estão instalados executando os seguintes comandos em um console do PowerShell com privilégios elevados:

    ```powershell
    Install-WindowsFeature Hyper-V, HostGuardian -IncludeManagementTools -Restart
    ```

2.  Baixe a imagem ISO de uma fonte confiável e armazene-a em seu servidor de virtualização ou em um compartilhamento de arquivos acessível ao seu servidor de virtualização.

3.  No computador de gerenciamento que executa o Windows Server versão 1709, instale a VM blindada Ferramentas de Administração de Servidor Remoto executando o seguinte comando:

    ```powershell
    Install-WindowsFeature RSAT-Shielded-VM-Tools
    ```

4.  Abra o **Gerenciador do Hyper-V** no computador de gerenciamento e conecte-se ao servidor de virtualização.
    Você pode fazer isso clicando em "conectar-se ao servidor..." no painel ações ou clicando com o botão direito do mouse em Gerenciador do Hyper-V e escolhendo "conectar-se ao servidor..." Forneça o nome DNS para o servidor Hyper-V e, se necessário, as credenciais necessárias para se conectar a ele.

5.  Usando o Gerenciador do Hyper-V, [Configure um comutador externo](https://docs.microsoft.com/windows-server/virtualization/hyper-v/get-started/create-a-virtual-switch-for-hyper-v-virtual-machines) no servidor de virtualização para que a VM do Linux possa acessar a Internet para obter atualizações.

6.  Em seguida, crie uma nova máquina virtual na qual instalar o sistema operacional Linux.
    No painel Ações, clique em **novo** > **máquina virtual** para abrir o assistente.
    Forneça um nome amigável para sua VM, como "modelos Linux" e clique em **Avançar**.

7.  Na segunda página do assistente, selecione **geração 2** para garantir que a VM seja provisionada com um perfil de firmware baseado em UEFI.

8.  Conclua o restante do assistente de acordo com suas preferências.
    Não use um disco diferencial para esta VM; os discos de modelo de VM blindada não podem usar discos diferenciais.
    Por fim, conecte a imagem ISO baixada anteriormente à unidade de DVD virtual para essa VM para que você possa instalar o sistema operacional.

9.  No Gerenciador do Hyper-V, selecione sua VM recém-criada e clique em **conectar...** no painel Ações para anexar a um console virtual da VM.
    Na janela exibida, clique em **Iniciar** para ativar a máquina virtual.

10. Prossiga com o processo de instalação para a distribuição do Linux selecionada.
    Embora cada distribuição do Linux use um assistente de instalação diferente, os requisitos a seguir devem ser atendidos para VMs que se tornarão discos de modelo de VM blindadas do Linux:

    - O disco deve ser particionado usando o layout GPT (tabela particionamento GUID)
    - A partição raiz deve ser criptografada com DM-cript. A frase secreta deve ser definida como **frase secreta** (todas as letras minúsculas). Essa senha será aleatória e a partição criptografada novamente quando uma VM blindada for provisionada.
    - A partição de inicialização deve usar o sistema de arquivos **ext2**

11. Depois que o sistema operacional Linux for totalmente inicializado e você tiver entrado, é recomendável instalar o kernel Linux-virtual e os pacotes dos serviços de integração do Hyper-V associados.
    Além disso, você desejará instalar um servidor SSH ou outra ferramenta de gerenciamento remoto para acessar a VM depois que ela for blindada.

    No Ubuntu, execute o seguinte comando para instalar estes componentes:

    ```bash
    sudo apt-get install linux-virtual linux-tools-virtual linux-cloud-tools-virtual linux-image-extra-virtual openssh-server
    ```

    No RHEL, execute o seguinte comando em vez disso:

    ```bash
    sudo yum install hyperv-daemons openssh-server
    sudo service sshd start
    ```

    E, no SLES, execute o seguinte comando:

    ```bash
    sudo zypper install hyper-v
    sudo chkconfig hv_kvp_daemon on
    sudo systemctl enable sshd
    ```

12. Configure seu sistema operacional Linux conforme desejado.
    Qualquer software que você instalar, as contas de usuário adicionadas e as alterações de configuração em todo o usuário que você fizer serão aplicadas a todas as VMs futuras criadas a partir desse disco de modelo.
    Você deve evitar salvar segredos ou pacotes desnecessários no disco.

13. Se você estiver planejando usar System Center Virtual Machine Manager para implantar suas VMs, instale o agente convidado do VMM para habilitar o VMM a especializar seu sistema operacional durante o provisionamento da VM.
    A especialização permite que cada VM seja configurada com segurança com diferentes usuários e chaves SSH, configurações de rede e etapas de instalação personalizadas.
    Saiba como [obter e instalar o agente convidado do VMM](https://docs.microsoft.com/system-center/vmm/vm-linux#install-the-vmm-guest-agent) na documentação do VMM.

14. Em seguida, [adicione o repositório de software do Microsoft Linux ao Gerenciador de pacotes](../../administration/linux-package-repository-for-microsoft-software.md).

15. Usando o Gerenciador de pacotes, instale o pacote lsvmtools que contém o Shim do carregador de VMs blindadas Linux, os componentes de provisionamento e a ferramenta de preparação de disco.

    ```bash
    # Ubuntu 16.04
    sudo apt-get install lsvmtools

    # SLES 12 SP2
    sudo zypper install lsvmtools

    # RHEL 7.3
    sudo yum install lsvmtools
    ```

14. Quando terminar de personalizar o SO Linux, localize o programa de instalação do lsvmprep em seu sistema e execute-o.
    
    ```bash
    # The path below may change based on the version of lsvmprep installed
    # Run "find /opt -name lsvmprep" to locate the lsvmprep executable
    sudo /opt/lsvmtools-1.0.0-x86-64/lsvmprep
    ```

15. Desligue sua VM.

16. Se você tiver criado qualquer ponto de verificação de sua VM (incluindo pontos de verificação automáticos criados pelo Hyper-V com a atualização dos criadores de outono do Windows 10), certifique-se de excluí-los antes de continuar.
    Os pontos de verificação criam discos diferenciais (. avhdx) que não têm suporte no assistente de disco de modelo.
    
    Para excluir pontos de verificação, abra o **Gerenciador do Hyper-V**, selecione sua VM, clique com o botão direito do mouse no ponto de verificação superior no painel pontos de verificação e clique em **excluir subárvore do ponto de verificação**.

    ![Excluir todos os pontos de verificação de sua VM de modelo no Gerenciador do Hyper-V](../media/Guarded-Fabric-Shielded-VM/delete-checkpoints-lsvm-template.png)

## <a name="protect-the-template-disk"></a>Proteger o disco de modelo

A VM que você preparou na seção anterior está quase pronta para ser usada como um disco de modelo de VM blindada do Linux.
A última etapa é executar o disco por meio do assistente de disco de modelo, que fará o hash e assinará digitalmente o estado atual das partições raiz e de inicialização.
O hash e a assinatura digital são verificados quando uma VM blindada é provisionada para garantir que nenhuma alteração não autorizada tenha sido feita nas duas partições entre a criação e a implantação do modelo.

### <a name="obtain-a-certificate-to-sign-the-disk"></a>Obter um certificado para assinar o disco

Para assinar digitalmente as medições de disco, você precisará obter um certificado no computador em que você executará o assistente de disco de modelo.
O certificado deve atender aos seguintes requisitos:

Propriedade de certificado | Valor obrigatório
---------------------|---------------
Algoritmo de chave | RSA
Tamanho mínimo da chave | 2048 bits
Algoritmo de assinatura | SHA256 (recomendado)
Uso de chave | Assinatura digital

Os detalhes sobre esse certificado serão mostrados aos locatários quando criarem seus arquivos de dados de blindagem e estiverem autorizando discos em que confiam.
Portanto, é importante obter esse certificado de uma autoridade de certificação mutuamente confiável por você e seus locatários.
Em cenários empresariais em que você é o hoster e o locatário, você pode considerar a emissão desse certificado de sua autoridade de certificação corporativa.
Proteja esse certificado com cuidado, pois qualquer pessoa em posse desse certificado pode criar novos discos de modelo confiáveis da mesma forma que seu disco de autenticidade.

Em um ambiente de laboratório de teste, você pode criar um certificado autoassinado com o seguinte comando do PowerShell:

```powershell
New-SelfSignedCertificate -Subject "CN=Linux Shielded VM Template Disk Signing Certificate"
```

### <a name="process-the-disk-with-the-template-disk-wizard-cmdlet"></a>Processar o disco com o cmdlet do assistente de disco de modelo

Copie o disco do modelo e o certificado para um computador executando o Windows Server, versão 1709 e execute os comandos a seguir para iniciar o processo de assinatura.
O VHDX que você fornecer ao parâmetro `-Path` será substituído pelo disco de modelo atualizado, portanto, certifique-se de fazer uma cópia antes de executar o comando.

> [!IMPORTANT]
> O Ferramentas de Administração de Servidor Remoto disponível no Windows Server 2016 ou Windows 10 não pode ser usado para preparar um disco de modelo de VM blindada do Linux.
> Use apenas o cmdlet [Protect-TemplateDisk](https://docs.microsoft.com/powershell/module/shieldedvmtemplate/protect-templatedisk?view=win10-ps) disponível no Windows Server, versão 1709 ou o ferramentas de administração de servidor remoto disponível no windows Server 2019 para preparar um disco de modelo de VM blindada do Linux.

```powershell
# Replace "THUMBPRINT" with the thumbprint of your template disk signing certificate in the line below
$certificate = Get-Item Cert:\LocalMachine\My\THUMBPRINT

Protect-TemplateDisk -Path 'C:\temp\MyLinuxTemplate.vhdx' -TemplateName 'Ubuntu 16.04' -Version 1.0.0.0 -Certificate $certificate -ProtectedTemplateTargetDiskType PreprocessedLinux
```

Seu disco de modelo agora está pronto para ser usado para provisionar VMs blindadas Linux.
Se você estiver usando System Center Virtual Machine Manager para implantar sua VM, agora você pode copiar o VHDX para a biblioteca do VMM.

Talvez você também queira extrair o catálogo de assinatura de volume do VHDX.
Esse arquivo é usado para fornecer informações sobre o certificado de autenticação, o nome do disco e a versão aos proprietários da VM que desejam usar seu modelo.
Eles precisam importar esse arquivo para o assistente de arquivo de dados de blindagem para autorizar você, o autor do modelo em posse do certificado de autenticação, para criar esse e futuros discos de modelo para eles.

Para extrair o catálogo de assinatura de volume, execute o seguinte comando no PowerShell:

```powershell
Save-VolumeSignatureCatalog -TemplateDiskPath 'C:\temp\MyLinuxTemplate.vhdx' -VolumeSignatureCatalogPath 'C:\temp\MyLinuxTemplate.vsc'
```
