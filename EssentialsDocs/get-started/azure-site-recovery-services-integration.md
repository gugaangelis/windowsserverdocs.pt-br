---
title: "Integração de serviços de recuperação de Site Azure"
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
ms.openlocfilehash: 85e681cfc19241941773dd94f05ba59759aaf62a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
#<a name="azure-site-recovery-services-integration"></a>Integração de serviços de recuperação de Site Azure

>Aplica-se a: Windows Server 2016 Essentials

[Serviços de recuperação de Site Azure](https://azure.microsoft.com/en-us/documentation/services/site-recovery/) é um serviço oferecido pela Microsoft Azure habilitando replicação em tempo real de suas máquinas virtuais (VM) para um backup cofre no Azure. Se seu site ou o servidor estiver desligado devido a um hardware ou outra falha, você pode failover ao Azure onde a imagem VM armazenada no seu backup cofre será configurada como uma VM em execução no Azure. Combinado com uma rede Virtual do Azure, no caso de um failover para Azure, cliente computadores conectados anteriormente para o servidor local transparente se conectará ao servidor em execução no Azure.

Integração de serviços de recuperação de Site do Azure com o Windows Server Essentials começa da mesma maneira como configurando [redes virtuais do Azure](azure-virtual-network-integration.md). Do **integração de serviços de nuvem do Microsoft** página no painel, clique em **integrar com serviços de recuperação de Site Azure** à direita do painel:

![Uma captura de tela mostrando a guia de Introdução na Home page do painel do Windows Server Essentials. Na guia de Introdução, a seção serviços foi selecionada e o painel indica em integração de serviços de nuvem do Microsoft Azure recuperação está desabilitada no momento.](media/azure-site-recovery-1.PNG)

Com a integração de rede Virtual do Azure e integração com o Backup do Azure, você deve fazer logon no Azure com a conta de Azure existente ou criar uma nova conta:

![Uma captura de tela mostrando a página de entrada ao Microsoft Azure do Assistente para habilitar replicação para Azure. O botão de entrada é exibido porque o usuário ainda não tiver entrado no Microsoft Azure.](media/azure-site-recovery-2.PNG)

Depois de fazer logon com êxito no Azure, você verá uma tela que pedirá que você gostaria de associar o serviço de recuperação de Site do Azure, bem como a região do Azure onde seu VM será armazenada e hospedada de assinatura:

![Uma captura de tela mostrando a página de entrada ao Microsoft Azure do Assistente para habilitar replicação para Azure. Porque o usuário tiver entrado no Microsoft Azure, esta página fornece opções para selecionar uma assinatura, conta de armazenamento e região.](media/azure-site-recovery-3.PNG)

Após a assinatura e seleção de região, uma nova guia aparecerá no **painel do Windows Server Essentials** chamado **Azure recuperação**. A varredura de rede é feita para identificar e enumerar os servidores de host com suporte (executando o Windows Server Hyper-V 2012 R2 e acima), bem como as máquinas virtuais (convidados) sob os hosts individuais:

![Uma captura de tela mostrando a página de recuperação do Azure do painel do Windows Server Essentials. Dois hosts Hyper-V são exibidos juntamente com as máquinas virtuais em execução nesses hosts. Uma máquina virtual denominado ramh157v01 no host RAM-H1-7 é selecionado e replicação ao Azure atualmente está desabilitada para essa máquina virtual.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Habilitando máquinas virtuais de convidado para proteção

Após a seleção de uma máquina virtual presente na janela de recuperação do Azure, você pode clicar em **habilitar replicação ao Azure** no lado direito do painel para preparar e copie a máquina virtual™ s imagem de inicialização ao Azure:

![Uma captura de tela mostrando a caixa de diálogo habilitar replicação para Azure. Uma barra de progresso é exibida como um host está sendo adicionado.](media/azure-site-recovery-5.PNG)

Durante esse processo, o agente do serviço de recuperação de Site do Azure é instalado no servidor host, um cofre backup onde a imagem do convidado da que VM será armazenada é criada e começa a replicação da imagem com o Azure. Dependendo do tamanho da VM está sendo replicado, a conclusão do processo de replicação pode levar horas ou dias. Até que a imagem inteira de VM e deltas mais recentes serão replicados para Azure, tarefas de failover não estão disponíveis e a VM não está protegida. Depois que a replicação for concluída, a coluna Status da proteção na janela do Azure recuperação será alterado de **Replicating** para **Enabled**:

![Uma captura de tela mostrando a página de recuperação do Azure do painel do Windows Server Essentials. Dois hosts Hyper-V são exibidos juntamente com as máquinas virtuais em execução nesses hosts. Uma máquina virtual denominado ramh12v02 no host RAM-H1-2 é selecionado e replicação ao Azure atualmente está habilitada para a máquina virtual.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Failover de um convidado da VM com o Azure

![Captura de tela mostrando o Failover VM à página Azure.](media/azure-site-recovery-7.PNG)

Quando uma máquina virtual que está protegida falha ou o servidor host que a execução da máquina virtual protegido falhe, finalizado com Falha ao Azure pode ser feita para manter a continuidade de negócios até no local virtual machine ou host servidor é reparado e disponível. Como a figura acima mostra, existem três tipos de failover que são compatíveis com os serviços de recuperação de Site do Azure:

-   **Testar Failover** plano de recuperação de desastres boa ƒA incorpora a capacidade para simular um desastre para garantir o tempo de inatividade mínimo no caso de um desastre real. Um Failover testar leva a imagem VM que tenha sido replicada para o Cofre de recuperação, provisiona-lo como uma máquina virtual em execução no Azure e permite que você se conectar ao servidor para testar cenários que se aplicam aos negócios. Durante um teste failover, a máquina virtual local continua a ser executado sem interrupções quanto não interromper negócios durante o teste de recuperação de desastres. Depois que o Failover teste for concluído, você irá parar o teste por meio do Portal do Azure, que provisiona cancela a máquina virtual e exclui o VHD. Durante o teste inteira de failover, a imagem VM em seu cofre recuperação continua a obter replicados da VM no local como se nada nunca aconteceu.

-   **Failover não planejado** ƒAn failover não planejado ocorre quando há uma falha de real com o servidor host protegido ou VM em execução no servidor host. O failover é disparada manualmente a partir de qualquer painel do Windows Server Essentials, ou se o servidor falhou em si é o servidor que Windows Server Essentials está em execução, pode ser disparada do Portal do Azure diretamente. Nesse caso, o Failover não planejado leva a imagem VM que tenha sido replicada para o Cofre de recuperação, provisiona-lo como uma máquina virtual em execução no Azure e permite que você se conectar ao servidor para testar cenários que se aplicam aos negócios. Quando o servidor é restaurado no local, um failback manual pode ser disparado do Portal do Azure que, em seguida, copia a imagem VM para baixo novamente para o servidor local.

-   **Planejado Failover** ƒA planejado failover é uma ação que pode ser executada que atividades planejadas, como manutenção de hardware, devem ocorrer que levaria o servidor para baixo. No caso de um failover planejado, o mesmo processo ocorre em relação o provisionamento de sua imagem VM replicada no Azure. No entanto, antes de fazer isso, o servidor local é desligado de forma ordenada para garantir que todas as alterações são replicadas ao Azure antes do desligamento. Depois que a manutenção planejada for concluída, você pode ativar manualmente um failback do painel do Windows Server Essentials ou o portal do Azure, que trazem-up do servidor local e, em seguida, recurso desprovisionará a VM no Azure e excluir o. Arquivo do VHD. Replicação de VM local ao Azure, em seguida, continuará a ocorrer novamente como normal.

Em qualquer um dos três casos acima, quando uma VM é failover para Azure, o painel do Windows Server Essentials mostrará a VM novos no Azure em execução como a figura a seguir.

![Uma captura de tela mostrando a página de recuperação do Azure do painel do Windows Server Essentials. Replicação ao Azure foi habilitada para um host chamado Essentials e uma máquina virtual nomeado Essentials-teste em execução no Azure indica que o host falhou ao longo para Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Consulte também
--------
[Introdução ao Windows Server Essentials](get-started.md)
