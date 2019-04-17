---
ms.assetid: a6343f1c-e9dd-4a02-91ad-39bd519d66cd
title: "Vários canais de SMB simplificada e redes de Cluster Multi-NIC"
ms.prod: windows-server-threshold
ms.technology: storage-failover-clustering
ms.topic: article
author: RobHindman
ms.author: robhind
ms.date: 09/15/2016
ms.openlocfilehash: 45d8364adf9d3db24a8e6d8f7bc91178ce7d1551
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="simplified-smb-multichannel-and-multi-nic-cluster-networks"></a>Vários canais de SMB simplificada e redes de Cluster Multi-NIC

> Aplica-se a: Windows Server (anual por canal), o Windows Server 2016

Simplificado SMB multicanal e Multi -<abbr title="placa de rede">NIC</abbr> redes de Cluster é um novo recurso no Windows Server 2016 que permite o uso de várias placas de rede na mesma sub-rede da rede cluster e habilita automaticamente a vários canais de SMB.  

Vários canais SMB e redes de Cluster Multi-NIC simplificada fornece os seguintes benefícios:  
- Clustering de failover reconhece automaticamente todas as NICs em nós que estão usando o mesmo switch / mesma sub-rede - nenhuma configuração adicional necessária.  
- Vários canais SMB é habilitada automaticamente.  
- Redes que têm recursos de endereços IP locais de Link IPv6 (fe80) apenas são reconhecidos no cluster somente redes (privadas).  
- Um único recurso de endereço IP é configurado em cada nome de rede de ponto de acesso do Cluster (CAP) (NN) por padrão.  
- Validação de cluster não são mais problemas de mensagens de aviso quando várias placas de rede são encontradas na mesma sub-rede.  

## <a name="requirements"></a>Requisitos  
-   Várias placas de rede por servidor, usando o mesmo switch / sub-rede.  

## <a name="how-to-take-advantage-of-multi-nic-clusters-networks-and-simplified-smb-multichannel"></a>Como tirar proveito do multi-NIC clusters de redes e vários canais SMB simplificada  
Esta seção descreve como tirar proveito das novas redes de clusters de multi-NIC e dos recursos de vários canais SMB simplificados no Windows Server 2016.  

### <a name="use-at-least-two-networks-for-failover-clustering"></a>Use pelo menos duas redes de cluster de Failover   
Embora seja raro, comutadores de rede podem falhar - é ainda melhor prática usar pelo menos duas redes de cluster de Failover. Todas as redes que são encontradas são usadas para de cluster. Evite usar uma única rede para o Cluster de Failover para evitar um único ponto de falha. Idealmente, deve haver vários caminhos de física de comunicação entre os nós no cluster e nenhum ponto de falha.  

![Ilustração de duas redes de cluster de Failover](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig1.png)  
**Figura 1: Use pelo menos duas redes de cluster de Failover**  

### <a name="use-multiple-nics-across-clusters"></a>Usar várias placas de rede nos clusters  

Máximo benefício do simplificada multicanal SMB é atingido quando várias placas de rede são usadas em clusters - no armazenamento e clusters de carga de trabalho de armazenamento. Isso permite que os clusters de carga de trabalho (Hyper-V, instância de Cluster de Failover do SQL Server, réplica de armazenamento, etc.) para usar os resultados e vários canais SMB em uso mais eficiente da rede. Convergido (também conhecido como disaggregated) configuração onde um cluster de servidor de arquivos fora de escala é usado para armazenar dados de carga de trabalho para um Hyper-V ou cluster instância de Cluster de Failover do SQL Server, essa rede geralmente é chamado "da sub-rede do Sul do Norte" do cluster / de rede. Muitos clientes maximizem a taxa de transferência da rede investindo em Opções e compatível com placas NIC RDMA.  

![Ilustração de uma sub-rede SMB do Sul do Norte](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig2.png)  
**Figura 2: Para obter a taxa de transferência máxima da rede, use várias placas de rede no cluster escalonadas servidor de arquivos e o cluster Hyper-V ou instância de Cluster de Failover do SQL Server - que compartilham a sub-rede do Sul do Norte**  

![Screencap de dois clusters usando várias placas de rede na mesma sub-rede para aproveitar os vários canais SMB](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig3.png)  
**Figura 3: Dois clusters (servidor de arquivos fora de escala para armazenamento, SQL Server <abbr title="instância de cluster de Failover">FCI</abbr> de carga de trabalho) usam várias placas de rede na mesma sub-rede para aproveitar vários canais SMB e obter a melhor taxa de transferência de rede.** 

## <a name="automatic-recognition-of-ipv6-link-local-private-networks"></a>Reconhecimento automático de redes privadas IPv6 Link Local  
Quando forem detectadas redes privadas (somente no cluster) com várias placas de rede, o cluster reconhecerá automaticamente endereços IP locais de Link IPv6 (fe80) para cada placa de rede em cada sub-rede. Esse tempo dos administradores salva desde que eles não têm mais configurar manualmente os recursos de endereço IP Local de Link IPv6 (fe80).  

Ao usar mais de uma rede privada (somente no cluster), verifique a configuração de roteamento IPv6 para garantir que o roteamento não está configurado para cruzar sub-redes, pois isso reduzirá o desempenho de rede.  

![Screencap de configuração automática de rede na UI do Gerenciador de Cluster de Failover](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig4.png)  
**Figura 4: Configuração do recurso de endereço IPv6 Link Local (fe80) automático**  

## <a name="throughput-and-fault-tolerance"></a>Taxa de transferência e tolerância  
Windows Server 2016 automaticamente detecta recursos NIC e tentará usar cada NIC na configuração mais rápida possível. Placas de rede que são usados em conjunto, NICs usando RSS e NICs com funcionalidade RDMA podem todos ser usadas. A tabela a seguir resume as compensações ao usar essas tecnologias. Taxa de transferência máxima é atingida quando usando várias RDMA compatível com placas de rede. Para obter mais informações, consulte [as Noções básicas do SMB multicanais](https://blogs.technet.microsoft.com/josebda/2012/06/28/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0/).

![Uma ilustração de tolerância de taxa de transferência e falhas de várias configurações de NIC](media/Simplified-SMB-Multichannel-and-Multi-NIC-Cluster-Networks/Clustering_MulitNIC_Fig5.png)  
**Figura 5: Taxa de transferência e tolerância a falhas para vários conifigurations NIC**   

## <a name="frequently-asked-questions"></a>Perguntas frequentes  
**Todas as NICs em uma rede de multi-NIC são usadas para palpitantes de coração de cluster?**  
    Sim.  

**Uma rede de multi-NIC pode ser usada para comunicação de cluster só? Ou ele só pode ser usado para comunicação de cliente e o cluster?**  
    A configuração funcionará - todas as funções de rede de cluster funcionará em uma rede de multi-NIC.  

**Vários canais SMB também é usado para o tráfego de CSV e cluster?**  
    Sim, por padrão todos os cluster e tráfego de CSV usará redes multi-NIC disponível. Os administradores podem usar os cmdlets do PowerShell Clustering de Failover ou Gerenciador de Cluster de Failover da interface do usuário para alterar a função de rede.  

**Como é possível ver as configurações de vários canais de SMB?**  
    Use o **Get-SMBServerConfiguration** cmdlet, procure o valor da propriedade EnableMultiChannel.  

**É a propriedade comuns de cluster que plumballcrosssubnetroutes respeitadas em uma rede de multi-NIC?**  
     Sim.  

## <a name="see-also"></a>Consulte também  
- [O que há de novo no Failover Clustering no Windows Server](whats-new-in-failover-clustering.md)  
