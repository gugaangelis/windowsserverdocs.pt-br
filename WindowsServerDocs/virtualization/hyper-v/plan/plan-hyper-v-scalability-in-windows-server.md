---
title: Planejar a escalabilidade do Hyper-V no Windows Server 2016
description: Lista o número máximo com suporte para os componentes que você pode adicionar ou remover do Hyper-V e de máquinas virtuais, como a quantidade de memória e quantos processadores virtuais.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 09/28/2016
ms.openlocfilehash: 3d94d8475f5de8d6b3d1d3f0bc549a8791e1d0c8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364065"
---
# <a name="plan-for-hyper-v-scalability-in-windows-server-2016"></a>Planejar a escalabilidade do Hyper-V no Windows Server 2016

> Aplica-se a: Windows Server 2016, Windows Server 2019
  
Este artigo fornece detalhes sobre a configuração máxima de componentes que você pode adicionar e remover em um host Hyper-V ou em suas máquinas virtuais, como processadores virtuais ou pontos de verificação. Ao planejar sua implantação, considere os máximos que se aplicam a cada máquina virtual, bem como os que se aplicam ao host do Hyper-V. 

Os máximos de memória e processadores lógicos são os maiores aumentos do Windows Server 2012, em resposta a solicitações para dar suporte a cenários mais novos, como aprendizado de máquina e análise de dados. O blog do Windows Server publicou recentemente os resultados de desempenho de uma máquina virtual com 5,5 terabytes de memória e 128 processadores virtuais que executam 4 TB de banco de dados na memória. O desempenho foi maior que 95% do desempenho de um servidor físico. Para obter detalhes, consulte [desempenho da VM em grande escala do Windows Server 2016 Hyper-V para processamento de transações na memória](https://blogs.technet.microsoft.com/windowsserver/2016/09/28/windows-server-2016-hyper-v-large-scale-vm-performance-for-in-memory-transaction-processing/). Outros números são semelhantes aos que se aplicam ao Windows Server 2012. o \(Maximums para Windows Server 2012 R2 era o mesmo que o Windows Server 2012. \) 
  
> [!NOTE]  
> Para obter informações sobre System Center Virtual Machine Manager (VMM), consulte [Virtual Machine Manager](https://technet.microsoft.com/system-center-docs/vmm/vmm). O VMM é um produto da Microsoft para o gerenciamento de um data center virtualizado que é vendido separadamente.  
  
## <a name="maximums-for-virtual-machines"></a>Máximo de máquinas virtuais  
Esses máximos se aplicam a cada máquina virtual. Nem todos os componentes estão disponíveis em ambas as gerações de máquinas virtuais. Para obter uma comparação das gerações, consulte [devo criar uma máquina virtual de geração 1 ou 2 no Hyper-V?](should-i-create-a-generation-1-or-2-virtual-machine-in-hyper-v.md) 
  
|Componente|Máximo|Observações|  
|-------------|-----------|---------|  
|Pontos de verificação|50|O número real pode ser menor, dependendo do armazenamento disponível. Cada ponto de verificação é armazenado como um arquivo. avhd que usa o armazenamento físico.|  
|Memória|12 TB para a geração 2; <br>1 TB para a geração 1|Analise os requisitos do sistema operacional específico para determinar os valores mínimos e recomendados.|  
|Portas seriais (COM)|2|nenhuma.|  
|Tamanho dos discos físicos conectados diretamente a uma máquina virtual|Varia|O tamanho máximo é determinado pelo sistema operacional convidado.|  
|Adaptadores do Fibre Channel Virtual|4|Como melhor prática, recomendamos que você conecte cada adaptador do Fibre Channel virtual a um SAN virtual diferente.|  
|Dispositivos de disquete virtuais|1 unidade de disquete|nenhuma.|
|Capacidade do disco rígido virtual|64 TB para o formato VHDX;<br>2040 GB para formato VHD|Cada disco rígido virtual é armazenado em mídia física como um arquivo .vhdx ou .vhd, dependendo do formato usado pelo disco rígido virtual.|  
|Discos IDE virtuais|4|O disco de inicialização (às vezes chamado de disco de inicialização) deve ser conectado a um dos dispositivos IDE. O disco de inicialização pode ser um disco rígido virtual ou um disco físico conectado diretamente a uma máquina virtual.|  
|Processadores virtuais|240 para geração 2;<br>64 para geração 1;<br>320 disponível para o sistema operacional do host (partição raiz)|O número de processadores virtuais com suporte em um sistema operacional convidado pode ser menor. Para obter detalhes, consulte as informações publicadas para o sistema operacional específico.|
|Controladores SCSI virtuais|4|O uso de dispositivos SCSI virtuais requer o Integration Services, que está disponível para sistemas operacionais convidados com suporte. Para obter detalhes sobre quais sistemas operacionais têm suporte, consulte [máquinas virtuais Linux e FreeBSD com](../Supported-Linux-and-FreeBSD-virtual-machines-for-Hyper-V-on-Windows.md) suporte e [sistemas operacionais convidados do Windows com suporte](../supported-windows-guest-operating-systems-for-hyper-v-on-windows.md).|  
|Discos SCSI virtuais|256|Cada controlador SCSI oferece suporte a até 64 discos, o que significa que cada máquina virtual pode ser configurada com até 256 discos SCSI virtuais (4 controladores x 64 discos por controlador).|  
|Adaptadores de rede virtuais|O Windows Server 2016 dá suporte ao total de 12:<br> -8 adaptadores de rede específicos do Hyper-V<br>-4 adaptadores de rede herdados <br> O Windows Server 2019 dá suporte ao total de 68: <br> -adaptadores de rede específicos do Hyper-V 64<br>-4 adaptadores de rede herdados  |O adaptador de rede específico do Hyper-V fornece um melhor desempenho e requer um driver incluído no Integration Services. Para obter mais informações, consulte [planejar a rede do Hyper-V no Windows Server](plan-hyper-v-networking-in-windows-server.md).|  
  
## <a name="maximums-for-hyper-v-hosts"></a>Máximos para hosts do Hyper-V  
Esses máximos se aplicam a cada host Hyper-V.  
  
|Componente|Máximo|Observações|  
|-------------|-----------|---------|  
|Processadores lógicos|512|Ambos devem ser habilitados no firmware:<br /><br />-Virtualização assistida por hardware<br />-DEP (prevenção de execução de dados) imposta por hardware<br /><br />O sistema operacional do host (partição raiz) verá apenas OS processadores lógicos máximos 320|  
|Memória|24 TB|nenhuma.|  
|Equipes de adaptador de rede (Agrupamento NIC)|Nenhum limite imposto pelo Hyper-V.|Para obter detalhes, consulte [agrupamento NIC](../../../networking/technologies/nic-teaming/NIC-Teaming.md).|  
|Adaptadores de rede físicos|Nenhum limite imposto pelo Hyper-V.|nenhuma.|  
|Máquinas virtuais em execução por servidor|1024|nenhuma.|  
|Armazenamento|Limitado pelo que é suportado pelo sistema operacional do host. Nenhum limite imposto pelo Hyper-V.|**Observação:** A Microsoft dá suporte ao NAS (armazenamento conectado à rede) ao usar o SMB 3,0. Não há suporte para armazenamento baseado em NFS.|
|Portas de comutador de rede virtual por servidor|Varia; nenhum limite imposto pelo Hyper-V.|O limite prático depende dos recursos computacionais disponíveis.|  
|Processadores virtuais por processador lógico|Nenhuma razão imposta pelo Hyper-V.|nenhuma.|  
|Processadores virtuais por servidor|2048|nenhuma.|  
|Redes SAN (redes de área de armazenamento) virtuais|Nenhum limite imposto pelo Hyper-V.|nenhuma.|  
|Comutadores virtuais|Varia; nenhum limite imposto pelo Hyper-V.|O limite prático depende dos recursos computacionais disponíveis.|  
 
## <a name="failover-clusters-and-hyper-v"></a>Clusters de Failover e Hyper-V  
Esta tabela lista os máximos que se aplicam ao usar o Hyper-V e o clustering de failover. É importante fazer o planejamento de capacidade para garantir que haverá recursos de hardware suficientes para executar todas as máquinas virtuais em um ambiente clusterizado.  

Para saber mais sobre as atualizações do clustering de failover, incluindo os novos recursos para máquinas virtuais, consulte [novidades no clustering de failover no Windows Server 2016](../../../failover-clustering/whats-new-in-failover-clustering.md).

|Componente|Máximo|Observações|  
|-------------|-----------|---------|  
|Nós por cluster|64|Considere o número de nós que deseja reservar para failover, bem como tarefas de manutenção como a aplicação de atualizações. Recomendamos que o planejamento inclua recursos suficientes para que um nó seja reservado para failover, o que significa que permanecerá ocioso até ser ativado pela falha de outro nó. (Esse nó às vezes é chamado de nó passivo.) Esse número pode ser aumentado se você quiser reservar nós adicionais. Não há razão ou multiplicador recomendado de nós reservados para nós ativos; o único requisito é que o número total de nós em um cluster não possa exceder o máximo de 64.|  
|Máquinas virtuais em execução por cluster e por nó|8\.000 por cluster|Vários fatores podem afetar o número real de máquinas virtuais que você pode executar ao mesmo tempo em um nó, como:<br />-Quantidade de memória física que está sendo usada por cada máquina virtual.<br />-Largura de banda de armazenamento e rede.<br />-Número de eixos de disco, o que afeta O desempenho de e/s de disco.|  
  

