---
title: VMs blindadas - Preparando uma VHD do auxiliar de blindagem de VM
ms.custom: na
ms.prod: windows-server-threshold
ms.topic: article
ms.assetid: 0e3414cf-98ca-4e91-9e8d-0d7bce56033b
manager: dongill
author: rpsqrd
ms.technology: security-guarded-fabric
ms.date: 08/29/2018
ms.openlocfilehash: 8e14cdeed435f23f28ca514e232fbcfa6220fc74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887717"
---
# <a name="shielded-vms---preparing-a-vm-shielding-helper-vhd"></a>VMs blindadas - Preparando uma VHD do auxiliar de blindagem de VM

>Aplica-se a: Windows Server 2019, Windows Server (canal semestral), Windows Server 2016

<!-- This comment creates a break between the Applies To above and the Important note below. -->

> [!IMPORTANT]
> Antes de começar estes procedimentos, certifique-se de que você instalou a atualização cumulativa mais recente do Windows Server 2016 ou estiver usando o Windows 10 mais recente [ferramentas de administração de servidor remoto](https://www.microsoft.com/en-us/download/details.aspx?id=45520). Caso contrário, os procedimentos não funcionará. 

Esta seção descreve as etapas executadas por um provedor de serviços de hospedagem para habilitar o suporte para converter VMs existentes em VMs blindadas.

Para entender como este tópico se encaixa no processo geral de implantação de VMs blindadas, consulte [hospedar as etapas de configuração de provedor de serviço para hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md).

## <a name="which-vms-can-be-shielded"></a>Qual as VMs podem ser blindadas?

O processo de blindagem para VMs existentes só está disponível para VMs que atendem aos seguintes pré-requisitos:

- O sistema operacional convidado é o Windows Server 2012, 2012 R2, 2016 ou um semestral versão do canal. As VMs do Linux existente não pode ser convertidas em VMs blindadas.
- A VM é uma geração 2 (firmware UEFI) de VM
- A VM não usa discos diferenciais para seu volume do sistema operacional.

## <a name="prepare-helper-vhd"></a>Preparar o VHD do auxiliar

1.  Em um computador com o Hyper-V e o recurso ferramentas de administração de servidor remoto **ferramentas de VM Blindada** instalado, crie uma nova geração 2 VM com um VHDX em branco e a instalação do Windows Server 2016 nele usando a instalação do ISO do Windows Server mídia. Essa VM não deve ser blindado e deve executar o Server Core ou Server com experiência Desktop.

    > [!IMPORTANT]
    > O VHD de auxiliar de blindagem de VM **não deve** estar relacionados a discos de modelo que você criou na [provedor de serviços de hospedagem cria um modelo de VM blindada](guarded-fabric-create-a-shielded-vm-template.md). Se você usar um disco de modelo novamente, haverá uma colisão de assinatura do disco durante o processo de blindagem porque ambos os discos terão o mesmo identificador do disco GPT. Você pode evitar isso criando um novo VHD (em branco) e instalando o Windows Server 2016 usando a mídia de instalação do ISO.

2.  Inicie a máquina virtual, conclua as etapas de instalação e faça logon na área de trabalho. Uma vez que você verificou que a VM está em um estado de funcionamento, desligue a VM.

3.  Em uma janela elevada do Windows PowerShell, execute o seguinte comando para preparar o VHDX criado anteriormente para se tornar um disco de auxiliar de blindagem de VM. Atualize o caminho com o caminho correto para o seu ambiente.

    ```powershell
    Initialize-VMShieldingHelperVHD -Path 'C:\VHD\shieldingHelper.vhdx'
    ```

4.  Depois que o comando foi concluído com êxito, copie o VHDX no compartilhamento de biblioteca do VMM. **Não** inicia a VM da etapa 1 novamente. Isso irá corromper o disco do auxiliar.

5.  Agora você pode excluir a VM da etapa 1 no Hyper-V.

## <a name="configure-vmm-host-guardian-server-settings"></a>Configurar configurações de servidor de guardião de Host do VMM

No Console do VMM, abra o painel de configurações e, em seguida **configurações do serviço guardião de Host** sob **geral**. Na parte inferior dessa janela, há um campo para configurar o local de seu VHD do auxiliar. Use o botão Procurar para selecionar o VHD do seu compartilhamento de biblioteca. Se você não vir seu disco no compartilhamento, talvez você precise atualizar manualmente a biblioteca no VMM para que ele apareça.

![VMM - configurações de serviço guardião de Host](../media/Guarded-Fabric-Shielded-VM/guarded-host-vmm-hgs-settings-01.png)

## <a name="see-also"></a>Consulte também

- [Etapas de configuração de provedor de serviço de hospedagem de hosts protegidos e VMs blindadas](guarded-fabric-configuration-scenarios-for-shielded-vms-overview.md)
- [Malha protegida e VMs blindadas](guarded-fabric-and-shielded-vms-top-node.md)
