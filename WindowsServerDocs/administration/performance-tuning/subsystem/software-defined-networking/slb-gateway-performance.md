---
title: Redes definidas do SLB Gateway ajuste de desempenho no Software
description: Diretrizes de rede SDN de ajuste de desempenho do Gateway de SLB
ms.prod: windows-server-threshold
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; AnPaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: fede7d404ddbb4f465eff435cc340db1907ce9d2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829927"
---
# <a name="slb-gateway-performance-tuning-in-software-defined-networks"></a>Redes definidas do SLB Gateway ajuste de desempenho no Software

O balanceamento de carga de software é fornecido por uma combinação de um Gerenciador de Balanceador de carga em VMs do controlador de rede, o comutador Virtual do Hyper-V e um conjunto de VMs Multixplexor de Balanceador de carga (Mux).

Nenhum ajuste de desempenho adicional é necessário para configurar o controlador de rede ou o host do Hyper-V para além do que de balanceamento de carga é descrito na [rede definida pelo Software](index.md) seção, a menos que você estiver usando o SR-IOV para o Muxes conforme descrito abaixo.

## <a name="slb-mux-vm-configuration"></a>Configuração de VM Mux SLB

Máquinas virtuais SLB Mux são implantadas em uma configuração ativo-ativo.  Isso significa que cada VM Mux implantada e adicionado ao controlador de rede possa processar solicitações de entrada.  Assim, a taxa de transferência agregada total de todas as conexões só é limitada pelo número de VMs Mux do que você implantou.  

Uma conexão individual para um IP Virtual (VIP) sempre será enviado para o mesmo Mux, supondo que o número de muxes permanece constante e, consequentemente, sua taxa de transferência será limitada à taxa de transferência de uma única VM Mux.  Muxes processar apenas o tráfego de entrada que é destinado a um VIP.  Pacotes de resposta vá diretamente da VM que está enviando a resposta para o comutador físico que encaminha para o cliente.

Em alguns casos quando a origem da solicitação se originar de um host SDN é adicionado ao mesmo controlador de rede que gerencia o VIP, otimização adicional do caminho para a solicitação de entrada também é executada que permite que a maioria dos pacotes viajem diretamente a partir de cliente para o servidor, ignorando a VM Mux inteiramente.  Nenhuma configuração adicional é necessária para essa otimização entrar em vigor.

Cada VM SLB Mux devem ser dimensionado de acordo com as diretrizes fornecidas na seção de requisitos de função de máquina virtual da infraestrutura do SDN do [planejar uma infraestrutura de rede definida pelo Software](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md) tópico.

## <a name="single-root-io-virtualization-sr-iov"></a>Virtualização de e/s de raiz única (SR-IOV)

Ao usar Ethernet de 40 Gbit, a capacidade para o comutador virtual para os pacotes de processo para a VM Mux torna-se o fator limitante para taxa de transferência de VM Mux.  Por isso é recomendável que a SR-IOV esteja ativado no adaptador de rede de VM de SLB VM para garantir que o comutador virtual não é o gargalo.

Para habilitar a SR-IOV, você deve habilitá-lo no comutador virtual ao comutador virtual é criado.  Neste exemplo, estamos criando um comutador virtual com SR-IOV e o switch embedded teaming (SET):
``` syntax
    new-vmswitch -Name SDNSwitch -EnableEmbeddedTeaming $true -NetAdapterName @("NIC1", "NIC2") -EnableIOV $true
```
Em seguida, ele deve ser habilitado nos adaptadores de rede virtual da VM SLB Mux que processam o tráfego de dados.  Neste exemplo, SR-IOV está sendo habilitada em todos os adaptadores:
``` syntax
    get-vmnetworkadapter -VMName SLBMUX1 | set-vmnetworkadapter -IovWeight 50
```
