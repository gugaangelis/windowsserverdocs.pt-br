---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: SMB Multichannel simplificado e redes de cluster de várias NICs
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 45d8364adf9d3db24a8e6d8f7bc91178ce7d1551
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881127"
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>SMB Multichannel simplificado e redes de cluster de várias NICs

> Aplica-se a: Windows Server (canal semestral), Windows Server 2016

SMB Multichannel e Multi - simplificados<abbr title="placa de Interface de rede">NIC</abbr> redes de Cluster é um novo recurso no Windows Server 2016 que permite o uso de várias NICs na mesma sub-rede de rede do cluster e habilita automaticamente o SMB Multichannel.  

SMB Multichannel simplificado e redes de Cluster Multi-NIC oferece os seguintes benefícios:  
- Clustering de failover reconhece automaticamente todas as NICs em nós que estão usando o mesmo comutador / mesmo subnet - nenhuma configuração adicional necessária.  
- O SMB Multichannel é habilitado automaticamente.  
- Redes que possuem apenas recursos de endereços IP locais de Link do IPv6 (fe80) são reconhecidos em cluster somente de redes (privadas).  
- Um único recurso de endereço IP é configurado em cada nome de rede do ponto de acesso do Cluster (CAP) (NN) por padrão.  
- Validação de cluster não emite mensagens de aviso quando várias NICs estiverem localizadas na mesma sub-rede.  

## <a name="requirements"></a>Requisitos  
-   Várias NICs por servidor, usando o mesmo comutador / sub-rede.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>Como tirar proveito de multi-NIC clusters redes e SMB multichannel simplificado  
Esta seção descreve como tirar proveito das novas redes de clusters com várias NICs e recursos de multicanal SMB simplificados no Windows Server 2016.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Use pelo menos duas redes para Clustering de Failover   
Embora seja raro, comutadores de rede podem falhar, é ainda melhor prática usar pelo menos duas redes para Clustering de Failover. Todas as redes encontradas são usadas para as pulsações do cluster. Evite usar uma única rede para o Cluster de Failover para evitar um ponto único de falha. Idealmente, deve haver vários caminhos de comunicação física entre os nós no cluster e nenhum ponto único de falha.  

![Ilustração de duas redes para Clustering de Failover](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**Figura 1: Use pelo menos duas redes para Clustering de Failover**  

### <a name="use-multiple-nics-across-clusters"></a>Usar várias NICs em clusters  

Benefício máximo do SMB multichannel simplificado é obtido quando várias NICs são usadas em clusters - em armazenamento e clusters de carga de trabalho de armazenamento. Isso permite que os clusters de carga de trabalho (Hyper-V, instância de Cluster de Failover do SQL Server, réplica de armazenamento, etc.) para usar o SMB multichannel e os resultados em um uso mais eficiente da rede. No convergida (também conhecida como desagregada) em que um cluster de servidor de arquivos de escalabilidade horizontal é usado para armazenar dados de carga de trabalho para um Hyper-V ou cluster de instância de Cluster de Failover do SQL Server, esta rede é frequentemente chamado de "a sub-rede do Norte-Sul" de configuração de cluster / rede . Muitos clientes maximizem taxa de transferência da rede com o investimento em switches e cartões de placa de rede compatíveis com RDMA.  

![Ilustração de uma sub-rede de SMB Norte-Sul](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**Figura 2: Para obter a taxa de transferência máxima da rede, usar várias NICs no cluster Hyper-V ou instância de Cluster de Failover do SQL Server – que compartilham a sub-rede do Norte-Sul e o cluster de servidor de arquivos de escalabilidade horizontal**  

![Captura de dois clusters usando várias NICs na mesma sub-rede para aproveitar o SMB multichannel](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**Figura 3: Dois clusters (servidor de arquivos de expansão para armazenamento, SQL Server <abbr title="instância de Clustering de Failover">FCI</abbr> para carga de trabalho) ambos usam várias NICs na mesma sub-rede para aproveitar o SMB Multichannel e alcançar melhor rede taxa de transferência.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Reconhecimento automático de redes privadas IPv6 Link Local  
Quando redes privadas (cluster) com várias NICs são detectadas, o cluster reconhecerá automaticamente endereços IP IPv6 Link Local (fe80) para cada NIC em cada sub-rede. Esse tempo dos administradores salva, pois eles não terão mais configurar manualmente os recursos de endereço IP IPv6 Link Local (fe80).  

Ao usar mais de uma rede privada (cluster), verifique a configuração de roteamento de IPv6 para garantir que o serviço de roteamento de mensagens não está configurado para várias sub-redes, uma vez que isso reduzirá o desempenho da rede.  

![Captura automática de configuração de rede na UI do Gerenciador de Cluster de Failover](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**Figura 4: Configuração automática do recurso de endereço IPv6 Link Local (fe80)**  

## <a name="throughput-and-fault-tolerance"></a>Taxa de transferência e tolerância a falhas  
Windows Server 2016 automaticamente detecta capacidades NIC e tentará usar a cada NIC na configuração mais rápida possível. NICs estão agrupadas, NICs usando RSS e NICs com capacidade RDMA podem todos ser usadas. A tabela a seguir resume os prós e contras ao usar essas tecnologias. Taxa de transferência máxima é obtida ao usar várias NICs de compatíveis com RDMA. Para obter mais informações, consulte [os conceitos básicos do SMB multicanais](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

![Uma ilustração da taxa de transferência e tolerância a falhas para várias configurações de NIC](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**Figura 5: Taxa de transferência e tolerância a falhas para vários conifigurations NIC**   

## <a name="frequently-asked-questions"></a>Perguntas frequentes  
**Todas as NICs em uma rede com várias NICs para palpitantes de núcleo de cluster são usadas?**  
    Sim.  

**Uma rede de multi-NIC pode ser usada para comunicação de cluster somente? Ou pode apenas ser usada para comunicação de cliente e o cluster?**  
    A configuração funcionará - todas as funções de rede de cluster funcionará em uma rede com várias NICs.  

**O SMB Multichannel também é usado para tráfego de CSV e o cluster?**  
    Sim, por padrão todos os cluster e tráfego de CSV usará redes disponíveis com várias NICs. Os administradores podem usar os cmdlets do PowerShell de Clustering de Failover ou o Gerenciador de Cluster de Failover da interface do usuário para alterar a função de rede.  

**Como posso ver as configurações de SMB Multichannel?**  
    Use o **Get-SMBServerConfiguration** cmdlet, procure o valor da propriedade EnableMultiChannel.  

**É a propriedade de cluster comuns que plumballcrosssubnetroutes respeitado em uma rede de multi-NIC?**  
     Sim.  

## <a name="see-also"></a>Consulte também  
- [O que há de novo no Clustering de Failover no Windows Server](whats-new-in-failover-clustering.md)  
