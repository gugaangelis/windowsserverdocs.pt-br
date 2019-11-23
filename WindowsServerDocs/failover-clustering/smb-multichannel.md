---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: SMB Multichannel simplificado e redes de cluster de várias NICs
ms.prod: windows-server
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 7816016daae1d06568cd6149791a9a368d8602f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361107"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>SMB Multichannel simplificado e redes de cluster de várias NICs

> Aplica-se a: Windows Server 2019, Windows Server 2016

SMB simplificado Multichannel e vários<abbr title="Placa de interface de rede">NIC</abbr> As redes de cluster são um recurso que habilita o uso de várias NICs na mesma sub-rede de rede de cluster e habilita automaticamente o SMB multicanal.

As redes de cluster SMB simplificadas e multicanais de vários NICs fornecem os seguintes benefícios:  
- O clustering de failover reconhece automaticamente todas as NICs em nós que estão usando o mesmo comutador/mesma sub-rede-nenhuma configuração adicional necessária.  
- O SMB multicanal é habilitado automaticamente.  
- Redes que têm apenas os recursos de endereços IP locais do link IPv6 (FE80) são reconhecidas em redes somente de cluster (privadas).  
- Um único recurso de endereço IP é configurado em cada nome de rede de ponto de acesso (CAP) do cluster por padrão.  
- A validação de cluster não emite mais mensagens de aviso quando várias NICs são encontradas na mesma sub-rede.  

## <a name="requirements"></a>Requisitos  
-   Várias NICs por servidor, usando o mesmo comutador/sub-rede.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>Como tirar proveito de redes de clusters com várias NICs e SMB de vários canais simplificados  
Esta seção descreve como aproveitar as novas redes de clusters de várias NICs e os recursos de vários canais SMB simplificados.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Usar pelo menos duas redes para clustering de failover   
Embora seja raro, os comutadores de rede podem falhar. ainda é melhor usar pelo menos duas redes para clustering de failover. Todas as redes encontradas são usadas para pulsações de cluster. Evite usar uma única rede para o cluster de failover a fim de evitar um ponto único de falha. O ideal é que haja vários caminhos de comunicação física entre os nós no cluster e nenhum ponto único de falha.  

![ilustração de duas redes para clustering de failover](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**Figura 1: usar pelo menos duas redes para clustering de failover**  

### <a name="use-multiple-nics-across-clusters"></a>Usar várias NICs em clusters  

O benefício máximo do SMB simplificado de Fibre Channel é obtido quando várias NICs são usadas em clusters – em clusters de carga de trabalho de armazenamento e armazenamento. Isso permite que os clusters de carga de trabalho (Hyper-V, SQL Server instância de cluster de failover, réplica de armazenamento etc.) usem o SMB multicanal e resulte em um uso mais eficiente da rede. Em uma configuração de cluster convergido (também conhecida como desagregada) em que um cluster de servidor de arquivos de escalabilidade horizontal é usado para armazenar dados de carga de trabalho para um cluster de instância de cluster de failover do Hyper-V ou SQL Server, essa rede é geralmente chamada de "sub-rede norte-sul"/rede. Muitos clientes maximizam a taxa de transferência dessa rede investindo em placas e comutadores NIC compatíveis com RDMA.  

![ilustração de uma sub-rede SMB Norte-Sul](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**Figura 2: para obter a taxa de transferência máxima da rede, use várias NICs no cluster do servidor de arquivos de escalabilidade horizontal e no cluster de instância do cluster de failover do Hyper-V ou do SQL Server-que compartilham a sub-rede norte-sul**  

![captura de dois clusters usando várias NICs na mesma sub-rede para aproveitar a](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png) SMB de vários canais  
**Figura 3: dois clusters (servidor de arquivos de escalabilidade horizontal para armazenamento, SQL Server instância <abbr title="clustering de failover">FCI</abbr> para carga de trabalho) use várias NICs na mesma sub-rede para aproveitar o SMB multicanal e obter uma melhor taxa de transferência de rede.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Reconhecimento automático de redes privadas locais do link IPv6  
Quando redes privadas (somente cluster) com várias NICs forem detectadas, o cluster reconhecerá automaticamente os endereços IP locais do link do IPv6 (FE80) para cada NIC em cada sub-rede. Isso poupa tempo dos administradores, pois eles não precisam mais configurar manualmente os recursos de endereço IP local do link do IPv6 (FE80).  

Ao usar mais de uma rede privada (somente cluster), verifique a configuração de roteamento IPv6 para garantir que o roteamento não esteja configurado para cruzar sub-redes, pois isso reduzirá o desempenho da rede.  

![captura da configuração de rede automática na interface do usuário do Gerenciador de Cluster de Failover](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**Figura 4: configuração de recurso de endereço local do link de IPv6 (FE80) automaticamente**  

## <a name="throughput-and-fault-tolerance"></a>Taxa de transferência e tolerância a falhas  
O Windows Server 2019 e o Windows Server 2016 detectam automaticamente os recursos da NIC e tentarão usar cada NIC na configuração mais rápida possível. NICs que são agrupadas, NICs usando RSS, e NICs com capacidade RDMA podem ser usadas. A tabela a seguir resume as compensações ao usar essas tecnologias. A taxa de transferência máxima é obtida ao usar várias NICs compatíveis com RDMA. Para obter mais informações, consulte [noções básicas do SMB Mutlichannel](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

![uma ilustração de taxa de transferência e tolerância a falhas para várias configurações de NIC](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**Figura 5: taxa de transferência e tolerância a falhas para vários conifigurations NIC**   

## <a name="frequently-asked-questions"></a>Perguntas frequentes  
**Todas as NICs em uma rede com várias NICs são usadas para a pulsação do coração do cluster?**  
    Sim.  

**Uma rede de várias NICs pode ser usada somente para comunicação de cluster? Ou ele só pode ser usado para comunicação de cliente e cluster?**  
    Qualquer configuração funcionará-todas as funções de rede de cluster funcionarão em uma rede de várias NICs.  

**O SMB Multichannel também é usado para o tráfego de CSV e de cluster?**  
    Sim, por padrão, todos os tráfegos de cluster e CSV usarão redes MultiNIC disponíveis. Os administradores podem usar os cmdlets do PowerShell de clustering de failover ou Gerenciador de Cluster de Failover interface do usuário para alterar a função de rede.  

**Como posso ver as configurações de Fibre Channel do SMB?**  
    Use o cmdlet **Get-SMBServerConfiguration** , procure o valor da propriedade EnableMultiChannel.  

**A propriedade comum do cluster PlumbAllCrossSubnetRoutes respeitada em uma rede de várias NICs?**  
     Sim.  

## <a name="see-also"></a>Consulte também  
- [O que há de novo no clustering de failover no Windows Server](whats-new-in-failover-clustering.md)  
