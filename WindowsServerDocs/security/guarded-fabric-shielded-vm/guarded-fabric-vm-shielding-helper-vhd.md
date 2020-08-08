---
title: VMs blindadas-preparando um VHD auxiliar de blindagem de VM
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.author: ryanpu
ms.date: 08/29/2018
ms.openlocfilehash: 2c6a74b3dd18465534a662e6c1afa37ac197aca6
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87943986"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>VMs blindadas-preparando um VHD auxiliar de blindagem de VM

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

> [!IMPORTANT]
> Antes de iniciar esses procedimentos, verifique se você instalou a atualização cumulativa mais recente do Windows Server 2016 ou está usando o Windows 10 [ferramentas de administração de servidor remoto](https://www.microsoft.com/download/details.aspx?id=45520)mais recente. Caso contrário, os procedimentos não funcionarão.

Esta seção descreve as etapas executadas por um provedor de serviços de hospedagem para habilitar o suporte para a conversão de VMs existentes em VMs blindadas.

Para entender como este tópico se encaixa no processo geral de implantação de VMs blindadas, consulte [as etapas de configuração do provedor de serviço de hospedagem para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="which-vms-can-be-shielded"></a>Quais VMs podem ser blindadas?

O processo de blindagem para VMs existentes só está disponível para VMs que atendem aos seguintes pré-requisitos:

- O sistema operacional convidado é o Windows Server 2012, 2012 R2, 2016 ou uma versão de canal semestral. As VMs do Linux existentes não podem ser convertidas em VMs blindadas.
- A VM é uma VM de geração 2 (firmware UEFI)
- A VM não usa discos diferenciais para seu volume de so.

## <a name="prepare-helper-vhd"></a>Preparar VHD do auxiliar

1.  Em um computador com o Hyper-V e o recurso Ferramentas de Administração de Servidor Remoto **ferramentas de VM blindadas** instaladas, crie uma nova VM de geração 2 com um VHDX em branco e instale o windows Server 2016 nele usando a mídia de instalação ISO do Windows Server. Essa VM não deve ser blindada e deve executar o Server Core ou o servidor com a experiência desktop.

    > [!IMPORTANT]
    > O VHD do auxiliar de blindagem de VM **não deve** estar relacionado aos discos de modelo que você criou no [provedor de serviços de hospedagem cria um modelo de VM blindada](guarded-fabric-create-a-shielded-vm-template.md). Se você reutilizar um disco de modelo, haverá uma colisão de assinatura de disco durante o processo de blindagem porque ambos os discos terão o mesmo identificador de disco GPT. Você pode evitar isso criando um novo VHD (em branco) e instalando o Windows Server 2016 nele usando a mídia de instalação ISO.

2.  Inicie a VM, conclua as etapas de configuração e faça logon na área de trabalho. Depois de verificar se a VM está em um estado de funcionamento, desligue a VM.

3.  Em uma janela do Windows PowerShell com privilégios elevados, execute o seguinte comando para preparar o VHDX criado anteriormente para se tornar um disco auxiliar de blindagem de VM. Atualize o caminho com o caminho correto para o seu ambiente.

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  Depois que o comando for concluído com êxito, copie o VHDX para o compartilhamento de biblioteca do VMM. **Não** inicie a VM da etapa 1 novamente. Isso corromperá o disco auxiliar.

5.  Agora você pode excluir a VM da etapa 1 no Hyper-V.

## <a name="configure-vmm-host-guardian-server-settings"></a>Definir configurações do servidor guardião de host do VMM

No console do VMM, abra o painel configurações e, em seguida, **hospede as configurações do serviço guardião** em **geral**. Na parte inferior desta janela, há um campo para configurar o local do seu VHD auxiliar. Use o botão procurar para selecionar o VHD do seu compartilhamento de biblioteca. Se você não vir o disco no compartilhamento, talvez seja necessário atualizar manualmente a biblioteca no VMM para que ela seja exibida.

![VMM-configurações do serviço guardião de host](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="additional-references"></a>Referências adicionais

- [Etapas de configuração do provedor de serviços de hospedagem para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
