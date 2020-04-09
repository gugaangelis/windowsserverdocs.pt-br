---
title: Ajuste de desempenho do gateway SLB em redes definidas pelo software
description: Diretrizes de ajuste de desempenho do gateway SLB em redes SDN
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; anpaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: d1a497f24eba26b3b9b866772ae5171ea0fcbb24
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851579"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>Ajuste de desempenho do gateway SLB em redes definidas pelo software

O balanceamento de carga de software é fornecido por uma combinação de um Gerenciador de balanceador de carga nas VMs do controlador de rede, do comutador virtual do Hyper-V e de um conjunto de VMs de Load Balancer Multixplexor (MUX).

Nenhum ajuste de desempenho adicional é necessário para configurar o controlador de rede ou o host Hyper-V para balanceamento de carga além do que está descrito na seção [rede definida pelo software](index.md) , a menos que você esteja usando Sr-IOV para o Muxes conforme descrito abaixo.

## <a name="slb-mux-vm-configuration"></a>Configuração de VM MUX SLB

As máquinas virtuais SLB MUX são implantadas em uma configuração ativo-ativo.  Isso significa que todas as VMs MUX implantadas e adicionadas ao controlador de rede podem processar solicitações de entrada.  Assim, a taxa de transferência total agregada de todas as conexões é limitada apenas pelo número de VMs MUX implantadas.  

Uma conexão individual com um VIP (IP virtual) sempre será enviada para o mesmo MUX, supondo que o número de Muxes permaneça constante e, como resultado, sua taxa de transferência será limitada à taxa de transferência de uma única VM Mux.  Muxes processa apenas o tráfego de entrada destinado a um VIP.  Os pacotes de resposta vão diretamente da VM que está enviando a resposta para o comutador físico que o encaminha para o cliente.

Em alguns casos, quando a origem da solicitação é originada de um host SDN que é adicionado ao mesmo controlador de rede que gerencia o VIP, a otimização adicional do caminho de entrada para a solicitação também é executada, o que permite que a maioria dos pacotes percorra diretamente do cliente para o servidor, ignorando totalmente a VM Mux.  Nenhuma configuração adicional é necessária para que essa otimização ocorra.

Cada VM MUX SLB deve ser dimensionada de acordo com as diretrizes fornecidas na seção requisitos de função de máquina virtual de infraestrutura SDN do tópico [planejar uma infraestrutura de rede definida pelo software](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md) .

## <a name="single-root-io-virtualization-sr-iov"></a>SR-IOV (virtualização de e/s de raiz única)

Ao usar o 40 Gbit Ethernet, a capacidade para o comutador virtual processar pacotes para a VM MUX torna-se o fator de limitação para a taxa de transferência da VM Mux.  Por isso, é recomendável que o SR-IOV seja habilitado no adaptador de rede VM da VM SLB para garantir que o comutador virtual não seja o afunilamento.

Para habilitar o SR-IOV, você deve habilitá-lo no comutador virtual quando o comutador virtual for criado.  Neste exemplo, estamos criando um comutador virtual com o switch Embedded Integration (SET) e SR-IOV:
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
Em seguida, ele deve ser habilitado no (s) adaptador (es) de rede virtual da VM SLB MUX que processa o tráfego de dados.  Neste exemplo, o SR-IOV está sendo habilitado em todos os adaptadores:
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
