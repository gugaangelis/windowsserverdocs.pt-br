---
title: Somente software (SO) recursos e tecnologias
description: Esses recursos são implementados como parte do sistema operacional e são independentes do NIC subjacente (s). Às vezes, esses recursos exigem algum ajuste da NIC para a operação ideal. Esses exemplos de recursos do Hyper-v como recursos de não-Hyper-V como agrupamento NIC, listas de controle de acesso (ACLs) e qualidade de serviço de máquina Virtual (vmQoS).
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: pashort
author: shortpatti
ms.date: 09/20/2018
ms.openlocfilehash: 504bc92971e778b468812dc4064fa6f0afff87ad
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823777"
---
# <a name="software-only-so-features-and-technologies"></a>Somente software (SO) recursos e tecnologias
Apenas recursos de software são implementados como parte do sistema operacional e são independentes do NIC subjacente (s). Às vezes, esses recursos exigem algum ajuste da NIC para a operação ideal. Esses exemplos de recursos do Hyper-v como recursos de não-Hyper-V como agrupamento NIC, listas de controle de acesso (ACLs) e qualidade de serviço de máquina Virtual (vmQoS).

## <a name="access-control-lists-acls"></a>Listas de controle de acesso (ACLs)

Um recurso para gerenciar a segurança de uma VM Hyper-V e SDNv1. Esse recurso se aplica a pilha não virtualizado do Hyper-V e a pilha de HVNv1. Você pode gerenciar o Hyper-V ACLs por meio de alternar [Add-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapteracl?view=win10-ps) e [Remove-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) cmdlets do PowerShell.

## <a name="extended-acls"></a>ACLs estendidas

ACLs estendidas do comutador Virtual Hyper-V permitem configurar o Hyper-V Virtual Switch estendido ACLs de porta para fornecer proteção de firewall e impor políticas de segurança para as VMs do locatário nos data centers. Como as ACLs de porta são configurados no comutador Virtual Hyper-V, em vez de dentro das VMs, o administrador pode gerenciar políticas de segurança para todos os locatários em um ambiente multilocatário.

Você pode gerenciar o comutador do Hyper-V estendido ACLs por meio de [Add-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapterextendedacl?view=win10-ps) e [Remove-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) cmdlets do PowerShell.

>[!TIP] 
>Esse recurso se aplica à pilha HNVv1. Para as ACLs na pilha de SDN, consulte a SDN Software Defined Networking) abaixo de ACLs.

Para obter mais informações sobre estendido porta Access Control Lists nessa biblioteca, consulte [criar políticas de segurança com estendido porta Access Control Lists](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/Create-Security-Policies-with-Extended-Port-Access-Control-Lists).

## <a name="nic-teaming"></a>Agrupamento NIC

O agrupamento NIC, também chamado de associação de NIC, é a agregação de várias portas NIC em uma entidade percebe que o host como uma única porta NIC. Agrupamento NIC protege contra a falha de uma única porta NIC (ou o cabo conectado a ele). Ele também agrega o tráfego de rede para a taxa de transferência mais rápida. Para obter mais detalhes, consulte [agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

Com o Windows Server 2016, você tem duas formas de fazer o agrupamento:

1.  Solução de equipe do Windows Server 2012

2.  Windows Server 2016 Switch Embedded agrupamento (SET)


## <a name="rsc-in-the-vswitch"></a>RSC em vSwitch

Receber segmento de união (RSC) em vSwitch é um recurso que usa pacotes que fazem parte do mesmo fluxo e chegam entre interrupções de rede e une-os em um único pacote antes de distribuí-los para o sistema operacional. O comutador virtual no Windows Server 2019 tem esse recurso. Para obter mais detalhes sobre esse recurso, consulte [receber união de segmentos no vSwitch](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).

## <a name="software-defined-networking-sdn-acls"></a>ACLs de rede (SDN) definida pelo software

A extensão de SDN no Windows Server 2016 aprimorado maneiras para dar suporte a ACLs. A pilha v2 de SDN do Windows Server 2016, ACLs de SDN são usadas em vez de ACLs e ACLs estendidas. Você pode usar o controlador de rede para gerenciar ACLs de SDN. 

## <a name="sdn-quality-of-service-qos"></a>SDN de qualidade de serviço (QoS)

A extensão SDN no Windows Server 2016 aprimorado maneiras de fornecer controle de largura de banda (reservas de egresso, limites de saída e limites de entrada) em uma base de 5 tuplas. Normalmente, essas políticas serão aplicadas no nível da vNIC ou vmNIC, mas você pode torná-los muito mais específicas. Na pilha v2 de SDN do Windows Server 2016, a QoS de SDN é usado em vez de vmQoS. Você pode usar o controlador de rede para gerenciar o QoS de SDN.

## <a name="switch-embedded-teaming-set"></a>Alternar agrupamento incorporado (SET)

CONJUNTO é uma solução alternativa do agrupamento NIC que você pode usar em ambientes que incluem o Hyper-V e a pilha de Software Defined Networking (SDN) no Windows Server 2016. CONJUNTO integra algumas funcionalidades de agrupamento NIC no comutador Virtual do Hyper-V. Para obter informações sobre agrupamento incorporado Switch nessa biblioteca, consulte [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="virtual-receive-side-scaling-vrss"></a>Virtual Receive Side Scaling (vRSS)

Software vRSS é usado para distribuir o tráfego destinado a uma VM entre vários processadores lógicos (LPs) da VM. O vRSS software oferece a VM a capacidade de lidar com mais tráfego de rede que um único LP seria capaz de lidar com. Para obter mais informações, consulte [Virtual Receive Side Scaling (vRSS)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top).

## <a name="virtual-machine-quality-of-service-vmqos"></a>Qualidade de serviço de máquina virtual (vmQoS)

Qualidade de serviço da máquina virtual é um recurso do Hyper-V que permite a alternância para definir limites sobre o tráfego gerado por cada VM. Ele também permite que uma VM reservar uma quantidade de largura de banda na conexão de rede externa, de modo que uma VM não pode enfraquecer a outra VM para a largura de banda. Na pilha v2 de SDN do Windows Server 2016, a QoS de SDN substitui vmQoS.

vmQoS pode definir limites de saída e as reservas de saída. Você deve determinar o modo de reserva de saída (peso relativo ou absoluta da largura de banda) antes de criar o comutador do Hyper-V.

-  Determine o modo de reserva de saída com o parâmetro – MinimumBandwidthMode do cmdlet New-VMSwitch PowerShell.

-  Defina o valor do limite de saída com o parâmetro – MaximumBandwidth sobre o cmdlet do PowerShell Set-VMNetworkAdapter.

-  Defina o valor para a reserva de saída com qualquer um dos seguintes parâmetros do cmdlet PowerShell de VMNetworkAdapter definido:

   -  Se o parâmetro – MinimumBandwidthMode sobre o cmdlet New-VMSwitch for absoluto, defina o parâmetro – MinimumBandwidthAbsolute sobre o cmdlet defina VMNetworkAdapter.

   -  Se o parâmetro – MinimumBandwidthMode sobre o cmdlet New-VMSwitch é o peso, defina o parâmetro – MinimumBandwidthWeight sobre o cmdlet defina VMNetworkAdapter.

Devido às limitações no algoritmo usado para esse recurso, é recomendável que o peso mais alto ou largura de banda absoluta não ser mais de 20 vezes o peso mais baixo ou largura de banda absoluta. Se for necessário mais controle, considere usar a pilha de SDN e o recurso de QoS de SDN.


---