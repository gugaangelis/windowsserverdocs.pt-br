---
title: Somente software (SO) recursos e tecnologias
description: Esses recursos são implementados como parte do sistema operacional e são independentes das NIC (s) subjacentes. Às vezes, esses recursos exigem algum ajuste da NIC para uma operação ideal. Exemplos disso incluem recursos do Hyper-v, como vmQoS (qualidade de serviço) de máquina virtual, ACLs (listas de controle de acesso) e recursos não Hyper-V como agrupamento NIC.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 0cafb1cc-5798-42f5-89b6-3ffe7ac024ba
manager: dougkim
ms.author: lizross
author: eross-msft
ms.date: 09/20/2018
ms.openlocfilehash: a83a36ce7a47f0ebde35bf93bdca20796dd37a28
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316886"
---
# <a name="software-only-so-features-and-technologies"></a>Somente software (SO) recursos e tecnologias
Os recursos somente de software são implementados como parte do sistema operacional e são independentes das NIC (s) subjacentes. Às vezes, esses recursos exigem algum ajuste da NIC para uma operação ideal. Exemplos disso incluem recursos do Hyper-v, como vmQoS (qualidade de serviço) de máquina virtual, ACLs (listas de controle de acesso) e recursos não Hyper-V como agrupamento NIC.

## <a name="access-control-lists-acls"></a>ACLs (listas de controle de acesso)

Um recurso do Hyper-V e do SDNv1 para gerenciar a segurança de uma VM. Esse recurso se aplica à pilha Hyper-V não virtualizada e à pilha HVNv1. Você pode gerenciar ACLs de comutador Hyper-V por meio de cmdlets do PowerShell [Add-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapteracl?view=win10-ps) e [Remove-VMNetworkAdapterAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) .

## <a name="extended-acls"></a>ACLs estendidas

ACLs estendidas do comutador virtual do Hyper-V permitem que você configure as ACLs de porta estendida do comutador virtual do Hyper-v para fornecer proteção de firewall e impor políticas de segurança para as VMs de locatário em data centers. Como as ACLs de porta são configuradas no comutador virtual do Hyper-V em vez de nas VMs, o administrador pode gerenciar políticas de segurança para todos os locatários em um ambiente multilocatário.

Você pode gerenciar ACLs estendidas do comutador Hyper-V por meio dos cmdlets do PowerShell [Add-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/add-vmnetworkadapterextendedacl?view=win10-ps) e [Remove-VMNetworkAdapterExtendedAcl](https://docs.microsoft.com/powershell/module/hyper-v/remove-vmnetworkadapteracl?view=win10-ps) .

>[!TIP] 
>Esse recurso se aplica à pilha HNVv1. Para ACLs na pilha SDN, consulte ACLs de rede definida pelo software SDN) abaixo.

Para obter mais informações sobre as listas de controle de acesso de porta estendida nesta biblioteca, consulte [criar políticas de segurança com listas de controle de acesso de porta estendida](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/Create-Security-Policies-with-Extended-Port-Access-Control-Lists).

## <a name="nic-teaming"></a>Agrupamento NIC

O agrupamento NIC, também chamado de acoplamento NIC, é a agregação de várias portas NIC em uma entidade que o host percebe como uma única porta NIC. O agrupamento NIC protege contra a falha de uma única porta NIC (ou o cabo conectado a ela). Ele também agrega o tráfego de rede para uma taxa de transferência mais rápida. Para obter mais detalhes, consulte [agrupamento NIC](https://docs.microsoft.com/windows-server/networking/technologies/nic-teaming/nic-teaming).

Com o Windows Server 2016, você tem duas maneiras de fazer o agrupamento:

1.  Solução de agrupamento do Windows Server 2012

2.  Agrupamento integrado do Windows Server 2016 switch (SET)


## <a name="rsc-in-the-vswitch"></a>RSC no vSwitch

A União de segmento de recebimento (RSC) no vSwitch é um recurso que usa pacotes que fazem parte do mesmo fluxo e chegam entre interrupções de rede e os une em um único pacote antes de entregá-los ao sistema operacional. O comutador virtual no Windows Server 2019 tem esse recurso. Para obter mais detalhes sobre esse recurso, confira [União de segmento de recebimento no vSwitch](https://docs.microsoft.com/windows-server/networking/technologies/hpn/rsc-in-the-vswitch).

## <a name="software-defined-networking-sdn-acls"></a>ACLs de SDN (rede definida pelo software)

A extensão SDN no Windows Server 2016 melhorou as maneiras de dar suporte a ACLs. Na pilha do Windows Server 2016 SDN v2, as ACLs de SDN são usadas em vez de ACLs e ACLs estendidas. Você pode usar o controlador de rede para gerenciar ACLs de SDN. 

## <a name="sdn-quality-of-service-qos"></a>QoS (Qualidade de Serviço) da SDN

A extensão SDN no Windows Server 2016 melhorou as maneiras de fornecer controle de largura de banda (reservas de egresso, limites de saída e limites de entrada) em uma base de 5 tuplas. Normalmente, essas políticas são aplicadas no nível vNIC ou vmNIC, mas você pode torná-las muito mais específicas. Na pilha do Windows Server 2016 SDN v2, o SDN QoS é usado em vez de vmQoS. Você pode usar o controlador de rede para gerenciar a QoS de SDN.

## <a name="switch-embedded-teaming-set"></a>Alternar equipe inserida (SET)

O conjunto é uma solução de agrupamento NIC alternativa que você pode usar em ambientes que incluem o Hyper-V e a pilha de SDN (rede definida pelo software) no Windows Server 2016. O conjunto integra algumas funcionalidades de agrupamento NIC ao comutador virtual Hyper-V. Para obter informações sobre o comutador incorporado na biblioteca, consulte [acesso remoto direto à memória (RDMA) e comutador inserido de equipe (Set)](https://docs.microsoft.com/windows-server/virtualization/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming).

## <a name="virtual-receive-side-scaling-vrss"></a>Virtual Receive Side Scaling (vRSS)

O vRSS de software é usado para distribuir o tráfego de entrada destinado a uma VM entre vários processadores lógicos (LPs) da VM. O vRSS de software dá à VM a capacidade de lidar com mais tráfego de rede do que um único LP seria capaz de lidar. Para obter mais informações, consulte [vRSS (recepção virtual)](https://docs.microsoft.com/windows-server/networking/technologies/vrss/vrss-top).

## <a name="virtual-machine-quality-of-service-vmqos"></a>VmQoS (qualidade de serviço de máquina virtual)

A qualidade de serviço da máquina virtual é um recurso do Hyper-V que permite que a mudança defina limites no tráfego gerado por cada VM. Ele também permite que uma VM Reserve uma quantidade de largura de banda na conexão de rede externa para que uma VM não possa ficar sem nenhuma outra VM para largura de banda. Na pilha do SDN v2 do Windows Server 2016, o SDN QoS substitui vmQoS.

vmQoS pode definir limites de saída e reservas de saída. Você deve determinar o modo de reserva de saída (peso relativo ou largura de banda absoluta) antes de criar o comutador do Hyper-V.

-  Determine o modo de reserva de saída com o parâmetro – MinimumBandwidthMode do cmdlet do PowerShell New-VMSwitch.

-  Defina o valor do limite de saída com o parâmetro – MaximumBandwidth no cmdlet Set-VMNetworkAdapter do PowerShell.

-  Defina o valor da reserva de saída com um dos seguintes parâmetros do cmdlet Set VMNetworkAdapter PowerShell:

   -  Se o parâmetro – MinimumBandwidthMode no cmdlet New-VMSwitch for absoluto, defina o parâmetro – MinimumBandwidthAbsolute no cmdlet Set VMNetworkAdapter.

   -  Se o parâmetro – MinimumBandwidthMode no cmdlet New-VMSwitch for Weight, defina o parâmetro – MinimumBandwidthWeight no cmdlet Set VMNetworkAdapter.

Devido às limitações no algoritmo usado para esse recurso, recomendamos que o peso mais alto ou a largura de banda absoluta não seja superior a 20 vezes o peso mais baixo ou a largura de banda absoluta. Se for necessário mais controle, considere usar a pilha SDN e o recurso SDN-QoS.


---