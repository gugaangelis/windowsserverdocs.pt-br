---
title: Integração de Serviços do Azure Site Recovery
description: Descreve como usar o Windows Server Essentials
ms.custom: na
ms.date: 10/01/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 94894cd9be06485bf842f96517172db709dbe93d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59848737"
---
#<a name="azure-site-recovery-services-integration"></a>Integração de Serviços do Azure Site Recovery

>Aplica-se a: Windows Server 2016 Essentials

[Serviços de recuperação de Site do Azure](https://docs.microsoft.com/azure/site-recovery/) é um serviço oferecido pelo Microsoft Azure, habilitando a replicação em tempo real de máquinas virtuais (VM) em um cofre de backup no Azure. Que seu servidor ou site está inativo devido a uma falha de hardware ou outra, você pode fazer o failover para o Azure em que a imagem VM armazenada no seu Cofre de backup será provisionada como uma VM em execução no Azure. Combinado com uma rede Virtual do Azure, no caso de um failover para o Azure, PCs conectado anteriormente ao servidor local do cliente será conectar de forma transparente para o servidor em execução no Azure.

A integração de serviços de recuperação de Site do Azure com o Windows Server Essentials começa da mesma maneira como configurando [rede Virtual do Azure](azure-virtual-network-integration.md). Dos **integração de serviços de nuvem do Microsoft** no painel, clique em **integrar com serviços do Azure Site Recovery** à direita do painel:

![Uma captura de tela mostrando a guia de Introdução na Home page do painel do Windows Server Essentials. Na guia de Introdução, a seção de serviços foi selecionada e indica o painel na integração de serviços de nuvem do Microsoft Azure Recovery está desabilitado no momento.](media/azure-site-recovery-1.PNG)

Assim como acontece com a integração de rede Virtual do Azure e integração de Backup do Azure, você deve fazer logon no Azure com a conta do Azure existente ou crie uma nova conta:

![Uma captura de tela mostrando a página do Assistente para habilitar a replicação do Azure para entrar no Microsoft Azure. O botão de entrada é exibido porque o usuário ainda não tiver entrado no Microsoft Azure.](media/azure-site-recovery-2.PNG)

Depois de fazer logon com êxito para o Azure, você verá uma tela que solicitará a assinatura que você deseja associar com o serviço de recuperação de Site do Azure, bem como a região do Azure em que a VM será armazenada e hospedada:

![Uma captura de tela mostrando a página do Assistente para habilitar a replicação do Azure para entrar no Microsoft Azure. Porque o usuário tiver entrado no Microsoft Azure, esta página fornece opções para selecionar uma assinatura, a conta de armazenamento e a região.](media/azure-site-recovery-3.PNG)

Após a assinatura e a seleção de região, uma nova guia aparecerá na **painel do Windows Server Essentials** chamado **Azure Recovery**. Verificação de rede é feito para identificar e enumerar os servidores de host com suporte (executando o Windows Server Hyper-V 2012 R2 e acima), bem como as máquinas virtuais (convidados) em hosts individuais:

![Uma captura de tela mostrando a página de recuperação do Azure do painel do Windows Server Essentials. Dois hosts Hyper-V são exibidos, juntamente com as máquinas virtuais em execução nesses hosts. Uma máquina virtual denominada ramh157v01 no host RAM-H1-7 está selecionada e a replicação para o Azure está desabilitada no momento para esta máquina virtual.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Habilitar máquinas virtuais de convidado para a proteção

Após a seleção de uma máquina virtual presente na janela de recuperação do Azure, você pode clicar em **habilitar a replicação para o Azure** no lado direito do painel para preparar e copiar a máquina virtual™ s imagem no Azure:

![Uma captura de tela mostrando a caixa de diálogo habilitar replicação para o Azure. Uma barra de progresso é exibida como um host que está sendo adicionado.](media/azure-site-recovery-5.PNG)

Durante esse processo, o agente de serviço do Azure Site Recovery é instalado no servidor de host, um cofre de backup em que a imagem do convidado que VM será armazenada é criada e começa a replicação da imagem para o Azure. Dependendo do tamanho da VM que está sendo replicado, a conclusão do processo de replicação pode levar horas ou dias. Até que a imagem VM inteira e mais recente deltas sejam replicadas para o Azure, tarefas de failover não estão disponíveis e a VM não está protegida. Depois que a replicação for concluída, a coluna de Status de proteção na janela de recuperação do Azure será alterado de **replicando** à **habilitado**:

![Uma captura de tela mostrando a página de recuperação do Azure do painel do Windows Server Essentials. Dois hosts Hyper-V são exibidos, juntamente com as máquinas virtuais em execução nesses hosts. Uma máquina virtual denominada ramh12v02 no host RAM-H1-2 é selecionado e a replicação para o Azure está habilitada para esta máquina virtual.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Failover de um convidado VM para o Azure

![Captura de tela mostrando o Failover de VM para a página do Azure.](media/azure-site-recovery-7.PNG)

Quando uma máquina virtual que é protegido falhar ou o servidor de host que as execuções de máquina virtual protegida no falhar, o failover para o Azure podem ser feitas para manter a continuidade de negócios até que o virtual machine ou host de servidor local é reparado e está disponível. Como a figura acima mostra, há três tipos de failover que são compatíveis com os serviços do Azure Site Recovery:

-   **Failover de teste** plano de recuperação de desastres boa ƒA incorpora a capacidade de simular um desastre para garantir que o tempo de inatividade mínimo em caso de desastre real. Um Failover de teste usa a imagem VM que tenha sido replicada para o Cofre de recuperação, provisiona a ele como uma máquina virtual em execução no Azure e permite que você se conecte ao servidor para testar os cenários que se aplicam aos negócios. Durante um failover de teste, a máquina virtual local continua a executar com sem interrupções para não afetem negócios durante o teste de recuperação de desastres. Quando o Failover de teste for concluído, você irá parar o teste por meio do Portal do Azure, que desprovisiona a máquina virtual e exclui o VHD. Durante o failover de teste inteira, a imagem de VM em seu Cofre de recuperação continuará para serem replicados da VM no local, como se nada nunca aconteceu.

-   **Failover não planejado** ƒAn failover não planejado ocorre quando há uma falha real com o servidor de host protegido ou a VM em execução no servidor host. O failover é disparado manualmente de um painel de controle do Windows Server Essentials, ou se o próprio servidor com falha é o servidor que Windows Server Essentials está em execução, pode ser disparado no Portal do Azure diretamente. Nesse caso, o Failover não planejado leva a imagem da VM que tenha sido replicada para o Cofre de recuperação, provisiona a ele como uma máquina virtual em execução no Azure e permite que você se conecte ao servidor para testar os cenários que se aplicam aos negócios. Quando o servidor é restaurado no local, um failback manual pode ser disparado do Portal do Azure que, em seguida, copiará a imagem da VM volta para o servidor local.

-   **Failover planejado** ƒA planejado failover é uma ação que pode ser executada no caso de atividades planejadas, como manutenção de hardware, devem ser realizadas que levaria o servidor para baixo. No caso de um failover planejado, o mesmo processo ocorre em relação ao provisionamento de sua imagem VM replicada no Azure. No entanto, antes de fazer isso, o servidor local é desligado de forma ordenada para garantir que todas as alterações são replicadas para o Azure antes do desligamento. Depois que a manutenção planejada for concluída, você pode acionar manualmente um failback do painel do Windows Server Essentials ou o portal do Azure, o que seria colocar para cima no servidor local e, em seguida, desprovisionar a VM no Azure e excluir o. Arquivo VHD. A replicação da VM local para o Azure, em seguida, continuará a ocorrer novamente como normal.

Em qualquer um dos três casos acima, quando uma VM é failover no Azure, o painel do Windows Server Essentials mostrará a nova VM no Azure em execução como na figura abaixo.

![Uma captura de tela mostrando a página de recuperação do Azure do painel do Windows Server Essentials. A replicação para o Azure foi habilitada para um host chamado Essentials e uma máquina virtual nomeada teste Essentials em execução no Azure indica que o host tem failover no Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Consulte também
--------
[Introdução ao Windows Server Essentials](get-started.md)
