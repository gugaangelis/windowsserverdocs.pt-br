---
title: Integração de Serviços do Azure Site Recovery
description: Descreve como usar o Windows Server Essentials
ms.date: 10/01/2016
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: geschuma
manager: mtillman
ms.openlocfilehash: aa84ae8f3d11631b76d2e85e6a50f309eb83dd74
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89622587"
---
# <a name="azure-site-recovery-services-integration"></a>Integração de Serviços do Azure Site Recovery

>Aplica-se a: Windows Server 2016 Essentials

Os [serviços de Azure site Recovery](/azure/site-recovery/) são um serviço oferecido pelo Microsoft Azure habilitando a replicação em tempo real de suas máquinas virtuais (VM) para um cofre de backup no Azure. Caso seu servidor ou site esteja inoperante devido a um hardware ou outra falha, você pode fazer failover para o Azure, onde a imagem de VM armazenada no cofre de backup será provisionada como uma VM em execução no Azure. Combinado com uma rede virtual do Azure, no caso de um failover para o Azure, os PCs cliente conectados anteriormente ao servidor local se conectarão de forma transparente ao servidor em execução no Azure.

A integração dos serviços de Azure Site Recovery com o Windows Server Essentials é iniciada da mesma maneira que a configuração da [rede virtual do Azure](azure-virtual-network-integration.md). Na página de **integração dos serviços de Microsoft Cloud** no painel, clique em **integrar com os serviços de Azure site Recovery** à direita do painel:

![Uma captura de tela mostrando a guia introdução na home page do painel do Windows Server Essentials. Na guia introdução, a seção serviços foi selecionada e o painel indica em integração de serviços de Microsoft Cloud que a recuperação do Azure está atualmente desabilitada.](media/azure-site-recovery-1.PNG)

Assim como ocorre com a integração de rede virtual do Azure e a integração de backup do Azure, você deve fazer logon no Azure com sua conta existente do Azure ou criar uma nova conta:

![Uma captura de tela mostrando a página entrar no Microsoft Azure do assistente para habilitar a replicação para o Azure. O botão entrar é exibido porque o usuário ainda não entrou no Microsoft Azure.](media/azure-site-recovery-2.PNG)

Depois de fazer logon com êxito no Azure, você verá uma tela que perguntará qual assinatura você deseja associar ao serviço de Azure Site Recovery, bem como a região do Azure onde sua VM será armazenada e hospedada:

![Uma captura de tela mostrando a página entrar no Microsoft Azure do assistente para habilitar a replicação para o Azure. Como o usuário entrou no Microsoft Azure, esta página fornece opções para selecionar uma assinatura, uma conta de armazenamento e uma região.](media/azure-site-recovery-3.PNG)

Após a seleção da assinatura e da região, uma nova guia será exibida no **painel do Windows Server Essentials** chamado **recuperação do Azure**. A verificação de rede é feita para identificar e enumerar os servidores de host com suporte (executando o Windows Server Hyper-V 2012 R2 e posterior), bem como as máquinas virtuais (convidados) nos hosts individuais:

![Uma captura de tela mostrando a página recuperação do Azure do painel do Windows Server Essentials. Dois hosts Hyper-V são exibidos junto com as máquinas virtuais em execução nesses hosts. Uma máquina virtual chamada ramh157v01 no host RAM-H1-7 está selecionada e a replicação para o Azure está atualmente desabilitada para esta máquina virtual.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Habilitando máquinas virtuais convidadas para proteção

Após a seleção de uma máquina virtual presente na janela de recuperação do Azure, você pode clicar em **habilitar replicação no Azure** no lado direito do painel para preparar e copiar a imagem da máquina virtual &trade; para o Azure:

![Uma captura de tela mostrando a caixa de diálogo Habilitar replicação para o Azure. Uma barra de progresso é exibida à medida que um host está sendo adicionado.](media/azure-site-recovery-5.PNG)

Durante esse processo, o agente de serviço Azure Site Recovery é instalado no servidor host, um cofre de backup no qual a imagem da VM convidada será armazenada e a replicação da imagem para o Azure começa. Dependendo do tamanho da VM que está sendo replicada, a conclusão do processo de replicação pode levar horas ou dias. Até que toda a imagem da VM e os deltas mais recentes sejam replicados para o Azure, as tarefas de failover não estarão disponíveis e a VM não será protegida. Depois que a replicação for concluída, a coluna status da proteção na janela recuperação do Azure será alterada de **replicando** para **habilitado**:

![Uma captura de tela mostrando a página recuperação do Azure do painel do Windows Server Essentials. Dois hosts Hyper-V são exibidos junto com as máquinas virtuais em execução nesses hosts. Uma máquina virtual chamada ramh12v02 no host RAM-H1-2 está selecionada e a replicação para o Azure está habilitada para esta máquina virtual no momento.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Failover de uma VM convidada para o Azure

![Captura de tela mostrando VM de failover para a página do Azure.](media/azure-site-recovery-7.PNG)

Quando uma máquina virtual que está protegida falhar ou o servidor host no qual a máquina virtual protegida for executada falhar, o failover para o Azure poderá ser feito para manter a continuidade dos negócios até que a máquina virtual local ou o servidor host seja reparado e disponível. Como mostra a figura acima, há três tipos de failover que têm suporte com os serviços de Azure Site Recovery:

-   **Failover de teste** ƒA bom plano de recuperação de desastre incorpora a capacidade de simular um desastre para garantir o tempo de inatividade mínimo no caso de um desastre real. Um failover de teste usa a imagem de VM que foi replicada em seu cofre de recuperação, a provisiona como uma máquina virtual em execução no Azure e permite que você se conecte ao servidor para testar cenários que se aplicam aos negócios. Durante um failover de teste, a máquina virtual local continua a ser executada sem interrupções para não interromper os negócios durante o teste de recuperação de desastre. Depois que o failover de teste for concluído, você interromperá o teste por meio do portal do Azure, que desprovisionará a máquina virtual e excluirá o VHD. Durante todo o failover de teste, a imagem de VM em seu cofre de recuperação continua a ser replicada da VM local como se nada já aconteceu.

-   **Failover não planejado** ƒAn failover não planejado ocorre quando há uma falha real com o servidor host protegido ou a VM em execução no servidor host. O failover é acionado manualmente do painel do Windows Server Essentials ou, se o próprio servidor com falha for o servidor no qual o Windows Server Essentials está sendo executado, poderá ser disparado diretamente no portal do Azure. Nesse caso, o failover não planejado usa a imagem de VM que foi replicada em seu cofre de recuperação, a provisiona como uma máquina virtual em execução no Azure e permite que você se conecte ao servidor para testar cenários que se aplicam aos negócios. Quando o servidor é restaurado localmente, um failback manual pode ser disparado no portal do Azure que, em seguida, copiará a imagem da VM de volta para o servidor local.

-   O **failover planejado ƒA failover** planejado é uma ação que pode ser executada no caso de atividades planejadas, como a manutenção de HW, devem ocorrer, o que levaria o servidor para baixo. No caso de um failover planejado, o mesmo processo ocorre com relação ao provisionamento de sua imagem de VM replicada no Azure. No entanto, antes de fazer isso, o servidor local é desligado de maneira ordenada para garantir que todas as alterações sejam replicadas no Azure antes do desligamento. Depois que a manutenção planejada for concluída, você poderá disparar um failback manualmente no painel do Windows Server Essentials ou na portal do Azure, o que trará o servidor local e, em seguida, desprovisionará a VM no Azure e excluirá o. Arquivo VHD. A replicação da VM local para o Azure continuaria a ocorrer novamente normalmente.

Em qualquer um dos três casos acima, quando uma VM passar por failover para o Azure, o painel do Windows Server Essentials mostrará a nova VM no Azure em execução como na figura abaixo.

![Uma captura de tela mostrando a página recuperação do Azure do painel do Windows Server Essentials. A replicação para o Azure foi habilitada para um host chamado Essentials e uma máquina virtual chamada Essentials-Test em execução no Azure indica que o host passou por failover no Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Confira também
--------
[Introdução ao Windows Server Essentials](get-started.md)