---
title: Etapa 2 servidores de Cluster de plano
description: Este tópico faz parte do guia de implantação de acesso remoto em um Cluster no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 673c5bfb-b590-4932-8e54-ca0a466d90cc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1f0ab7c1d888466ed575ab7cc2ed204db9a02542
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854407"
---
# <a name="step-2-plan-cluster-servers"></a>Etapa 2 servidores de Cluster de plano

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois de implantar um único servidor de acesso remoto, planeja adicionar mais servidores no cluster.  
  
|Tarefa|Descrição|  
|----|--------|  
|[2.1 Instalando funções e recursos](#BKMK_Install).|Para cada servidor que será adicionado ao cluster, planejar a instalação da função acesso remoto e o recurso NLB do Windows (se necessário), planeje a topologia, o endereçamento IP, roteamento e encaminhamento.|  
|[2.2 configurações do servidor](#BKMK_Config)|Defina configurações para cada servidor que será adicionado ao cluster. Observe que você pode configurar um cluster com balanceamento de carga dos servidores usando máquinas virtuais. Para conectividade e roteamento funcione corretamente, você deve configurar as máquinas virtuais para usar a falsificação de endereço MAC.|  
  
## <a name="BKMK_Install"></a>2.1 Instalando funções e recursos  
Para cada servidor que você deseja ingressar no cluster, planeje instalar a função de acesso remoto. Além disso, planeje instalar o recurso de balanceamento de carga rede (NLB), se você quiser balancear o tráfego para o cluster usando NLB do Windows. Para obter mais informações, consulte [balanceamento de carga de rede](https://technet.microsoft.com/windows-server-docs/networking/technologies/network-load-balancing).  
  
## <a name="BKMK_Config"></a>2.2 configurações do servidor  
Para cada servidor que será adicionado ao cluster, planeje as configurações de domínio e de endereço IP. Observe o seguinte:  
  
1.  Todos os servidores no cluster devem pertencer ao mesmo domínio.  
  
2.  Os servidores no cluster devem estar localizados na mesma sub-rede.  
  
3.  Cada servidor no cluster deve ter o mesmo número de adaptadores de rede em uso para a implantação do DirectAccess.  
  
Quando você balancear o cluster usando NLB do Windows são aplicadas as seguintes configurações de NLB do Windows:  
  
1.  Operação modo Unicast. Isso pode ser alterado para multicast usando o Gerenciador NLB. Essa configuração não pode ser modificada no console de gerenciamento de acesso remoto.  
  
2.  Carregar o peso, fator-definido como igual, em que todos os servidores de cluster têm carga igual.  
  
3.  Filtragem de tráfego de modo será ser com balanceamento de carga em vários hosts.  
  
4.  Afinidade de afinidade para um único é definida.  
  
5.  Os protocolos  

