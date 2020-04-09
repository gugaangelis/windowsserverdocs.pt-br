---
title: Ajuste de desempenho de redes definidas pelo software
description: Diretrizes de ajuste de desempenho da SDN (rede definida pelo software)
ms.prod: windows-server
ms.technology: performance-tuning-guide
ms.topic: article
ms.author: grcusanz; anpaul
author: phstee
ms.date: 10/16/2017
ms.openlocfilehash: cfd166aab7a0ef0383fe4700fdf50cc6d35289b1
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80851609"
---
# <a name="performance-tuning-software-defined-networks"></a>Ajuste de desempenho de redes definidas pelo software

A SDN (rede definida pelo software) no Windows Server 2016 é composta por uma combinação de um controlador de rede, hosts Hyper-V, gateways de balanceador de carga de software e gateways HNV.  Para ajustar cada um desses componentes, confira as seguintes seções:

## <a name="network-controller"></a>Controlador de rede

O controlador de rede é uma função do Windows Server que deve ser habilitada em máquinas virtuais em execução em hosts configurados para usar a SDN e controlados pelo controlador de rede.

Três VMs habilitadas para controlador de rede são suficientes para alta disponibilidade e máximo desempenho.  Cada VM deve ser dimensionada de acordo com as diretrizes fornecidas na seção de requisitos de função de máquina virtual da infraestrutura da SDN do tópico [Planejar uma infraestrutura de rede definida pelo software](../../../../networking/sdn/plan/Plan-a-Software-Defined-Network-Infrastructure.md).

### <a name="sdn-quality-of-service-qos"></a>QoS (Qualidade de Serviço) da SDN

Para verificar se o tráfego da máquina virtual é priorizado com eficiência e igualmente, é recomendável configurar a QoS da SDN nas máquinas virtuais de carga de trabalho.  Para saber mais sobre a configuração da QoS da SDN, confira o tópico [Configurar QoS para um adaptador de rede de VMs de locatário](../../../../networking/sdn/manage/Configure-QoS-for-Tenant-VM-Network-Adapter.md).

## <a name="hyper-v-host-networking"></a>Rede de hosts Hyper-V

As diretrizes fornecidas na seção de desempenho de E/S de rede de Hyper-V do guia [Ajuste de desempenho para servidores Hyper-V](../../role/remote-desktop/session-hosts.md) aplicam-se quando a SDN é usada. No entanto, essa seção aborda outras diretrizes que devem ser seguidas para garantir o melhor desempenho ao usar a SDN.

### <a name="physical-network-adapter-nic-teaming"></a>Agrupamento do adaptador de rede física (NIC)

Para ter o melhor desempenho e as melhores funcionalidades de failover, é recomendável configurar os adaptadores de rede física para serem agrupados.  Ao usar a SDN, é necessário criar a equipe com o SET (Agrupamento Incorporado de Comutador).  

O número ideal de membros da equipe é dois, porque o tráfego virtualizado será espalhado entre os dois membros da equipe para direções de entrada e de saída.  É possível ter mais de dois membros de equipe; no entanto, o tráfego de entrada será espalhado em, no máximo, dois dos adaptadores.  O tráfego de saída sempre será distribuído entre todos os adaptadores se o padrão de balanceamento de carga dinâmica permanecer configurado no comutador virtual.


### <a name="encapsulation-offloads"></a>Descarregamentos de encapsulamento

A SDN depende do encapsulamento de pacotes para virtualizar a rede.  Para ter um desempenho ideal, é importante que o adaptador de rede seja compatível com o descarregamento de hardware para o formato de encapsulamento usado.  Não há nenhum benefício significativo de desempenho de um formato de encapsulamento sobre outro.  O formato de encapsulamento padrão quando o controlador de rede é usado é VXLAN.

É possível determinar qual formato de encapsulamento está sendo usado por meio do controlador de rede com o seguinte cmdlet do PowerShell:

``` syntax
    (Get-NetworkControllerVirtualNetworkConfiguration -connectionuri $uri).properties.networkvirtualizationprotocol
```

Para ter o melhor desempenho, se VXLAN for retornado, verifique se seus adaptadores de rede física são compatíveis com o descarregamento da tarefa VXLAN.  Se NVGRE for retornado, então os adaptadores de rede física deverão dar suporte ao descarregamento da tarefa NVGRE.

### <a name="mtu"></a>MTU

O encapsulamento resulta na adição de bytes extras a cada pacote.  Para evitar a fragmentação desses pacotes, a rede física deverá ser configurada para usar quadros jumbo.  Um valor MTU de 9234 é o tamanho recomendado para VXLAN ou NVGRE e deve ser configurado no comutador físico para as interfaces físicas das portas de host (L2) e das interfaces do roteador (L3) das VLANs por meio das quais os pacotes encapsulados serão enviados.  Isso inclui as redes de trânsito, de provedor HNV e de gerenciamento.

O MTU no host Hyper-V é configurado por meio do adaptador de rede, e o Agente de Host do Controlador de Rede em execução no host Hyper-V adequará a sobrecarga de encapsulamento automaticamente se houver suporte do driver de adaptador de rede.  

Após o tráfego sair da rede virtual por meio de um gateway, o encapsulamento será removido e o MTU original conforme enviado da VM será usado.

### <a name="single-root-io-virtualization-sr-iov"></a>SR-IOV (virtualização de E/S de raiz única)

A SDN é implementada no host Hyper-V usando uma extensão de comutador de encaminhamento no comutador virtual.  Para essa extensão do comutador processar pacotes, a SR-IOV não deverá ser usada nas interfaces de rede virtual configuradas para uso com o controlador de rede, pois ela faz o tráfego de VM ignorar o comutador virtual.

A SR-IOV ainda poderá ser habilitada no comutador virtual, se desejado, e poderá ser usada pelos adaptadores de rede de VMs não controlados pelo controlador de rede.  Essas VMs da SR-IOV podem coexistir no mesmo comutador virtual como VMs controladas por controlador de rede que não usam a SR-IOV.

Se você está usando adaptadores de rede de 40 Gbit, é recomendável habilitar a SR-IOV no comutador virtual para gateways SLB (balanceamento de carga de software) para atingir a máxima taxa de transferência.  Isso é abordado mais detalhadamente na seção [Gateways de balanceador de carga de software](slb-gateway-performance.md).

## <a name="hnv-gateways"></a>Gateways HNV

É possível encontrar informações sobre o ajuste de gateways HNV para uso com a SDN na seção [Gateways HNV](hnv-gateway-performance.md).

## <a name="software-load-balancer-slb"></a>SLB (Balanceador de Carga de Software)

Os gateways SLB só podem ser usados com o controlador de rede e com a SDN.  É possível encontrar mais informações sobre como ajustar a SDN para uso com gateways SLB na seção [Gateways do Balanceador de Carga de Software](slb-gateway-performance.md).
