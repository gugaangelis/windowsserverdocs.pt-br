---
title: Use dispositivos virtuais de rede em uma rede Virtual
description: Este tópico faz parte do Software de rede definidos guia sobre como gerenciar as cargas de trabalho de locatário e redes virtuais no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: pashort
author: shortpatti
ms.openlocfilehash: db46189931263d230f013431f319eb2497589dee
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Use dispositivos virtuais de rede em uma rede Virtual

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber como implantar dispositivos de rede virtual no locatário redes virtuais.

Você pode adicionar dispositivos virtuais de rede para redes que realizam o espelhamento de funções de porta e roteamento definida pelo usuário.

Este tópico contém as seções a seguir.

- [Tipos de dispositivos de rede Virtual](#bkmk_types)
- [Implantando um dispositivo de rede Virtual](#bkmk_deploy)
- [Exemplo: Definido pelo usuário roteamento](#bkmk_routing)
- [Exemplo: Espelhamento de porta](#bkmk_port)

## <a name="bkmk_types"></a>Tipos de dispositivos de rede Virtual

Existem dois tipos de dispositivos virtuais que você pode usar em redes virtuais:

1. **Roteamento definido pelo usuário**. Roteamento definida pelo usuário substitui roteadores distribuídos em uma rede virtual com os recursos de roteamento do aparelho virtual.  Com definida pelo usuário de roteamento, o dispositivo virtual é usado como um roteador entre as sub-redes virtuais em uma rede virtual.
2. **Espelhamento de porta**. Com o espelhamento de porta, todo o tráfego de rede inserir ou deixar a porta monitorada é duplicado e enviado para um dispositivo virtual para análise. 
## <a name="bkmk_deploy"></a>Implantando um dispositivo de rede Virtual

Para implantar um dispositivo virtual, você deve primeiro criar uma máquina virtual (VM) que contém o dispositivo e conecte a VM para as sub-redes de rede virtual apropriado.

>[!NOTE]
>Para obter mais informações, consulte [criar um locatário VM e conectar-se a uma rede Virtual de locatário ou VLAN](Create-a-Tenant-VM.md)

Alguns dispositivos requerem vários adaptadores de rede virtual. Geralmente um adaptador de rede é dedicado para o gerenciamento de dispositivo enquanto adaptadores adicionais são usados para processar o tráfego. 

Se o seu aparelho requer vários adaptadores de rede, você deve criar cada interface de rede no controlador de rede. 

Você também deve atribuir uma ID de interface em cada host para cada um dos adaptadores adicionais que estão em várias sub-redes virtuais.

Depois que você concluiu a implantação de dispositivo virtual de rede, você pode usar o dispositivo para roteamento definida pelo usuário, o espelhamento de porta ou ambos.

##<a name="bkmk_routing"></a>Exemplo: Definido pelo usuário roteamento

Para a maioria dos ambientes, precisará apenas as rotas de sistema já definidas pelo roteador distribuído da rede virtual. No entanto, você talvez precise criar uma tabela de rota e adicionar uma ou mais rotas em casos específicos, como:

* Forçar encapsulamento à Internet por meio de sua rede local.
* Uso de dispositivos virtuais em seu ambiente.

Para esses cenários, você deve criar uma tabela de rota e adicione rotas definida pelo usuário para a tabela. Você pode ter várias tabelas de rota e a tabela de rota mesmo pode ser associada a uma ou mais sub-redes. 

Cada sub-rede só pode ser associado a uma tabela única rota. Todas as VMs em uma sub-rede usam a tabela de rota que está associada à sub-rede.

Sub-redes dependem de rotas de sistema até que uma tabela de rota é associada à sub-rede. Depois que existe uma associação, roteamento é feito com base na correspondência de prefixo mais longa (LPM) entre rotas definida pelo usuário e rotas do sistema. 

Se houver mais de uma rota com a mesma correspondência LPM, em seguida, a rota definido do usuário é selecionada primeiro - antes da rota de sistema. 

###<a name="step-1-create-the-route-table-properties"></a>Etapa 1: Criar a rota propriedades da tabela

Esta tabela de rota irá conter todas as rotas definida pelo usuário.  Rotas de sistema ainda se aplicarão acordo com as regras definidas acima.

Você pode usar os comandos de exemplo a seguir para criar propriedades de tabela de rota.

    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties

###<a name="step-2-add-a-route-to-the-route-table-properties"></a>Etapa 2: Adicionar uma rota para as propriedades de tabela de rota

Esta rota diz que todo o tráfego destinado a sub-rede 12.0.0.0/8 deve obter enviado ao dispositivo virtual na 192.168.1.10 fossem roteados.  É importante que o dispositivo tem um adaptador de rede virtual conectado à rede virtual com esse IP atribuído a uma interface de rede.

Você pode usar os comandos de exemplo a seguir para adicionar uma rota para as propriedades de tabela de rota.

    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route

Você pode adicionar rotas adicionais repetindo essa etapa para cada rota que você deseja definir.
s
###<a name="step-3-add-the-route-table-to-network-controller"></a>Etapa 3: Adicionar a tabela de rota para o controlador de rede
Você pode usar os comandos de exemplo a seguir para adicionar a tabela de rota para o controlador de rede.

    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties

###<a name="step-4-apply-the-route-table-to-the-virtual-subnet"></a>Etapa 4: Aplicar a tabela de rota à sub-rede virtual
 
Quando você aplica a tabela de rota à sub-rede virtual, a primeira sub-rede virtual na rede Tenant1_Vnet1 usa a tabela de rota. Você pode atribuir a tabela de rota para o máximo de sub-redes na rede virtual que desejar.

Você pode usar os comandos de exemplo a seguir para aplicar a tabela de rota à sub-rede virtual.

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid

Assim que você aplique a tabela de rota para a rede virtual, o tráfego é encaminhado para o dispositivo virtual. Você deve configurar a tabela de roteamento no dispositivo virtual para encaminhar o tráfego, de maneira que seja adequado para seu ambiente.

##<a name="bkmk_port"></a>Exemplo: Espelhamento de porta

Este exemplo permite que você configure o tráfego do MyVM_Ethernet1 para que o tráfego é espelhado para Appliance_Ethernet1.

Este exemplo pressupõe que você já tiver implantado o duas VMs, uma como o dispositivo e uma como a VM monitorar com espelhamento.

É importante que o dispositivo tem uma segunda interface de rede para gerenciamento porque depois espelhamento é ativado como um destino na Appliance_Ethernet1, ele não receberá mais tráfego destinado a interface IP configurada ali.

###<a name="step-1-get-the-virtual-network-on-which-your-vms-are-located"></a>Etapa 1: Obter a rede virtual no qual seu VMs estão localizados
Você pode usar o comando de exemplo a seguir para obter a rede virtual.

    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"

###<a name="step-2-get-the-network-controller-network-interfaces-for-the-mirroring-source-and-destination"></a>Etapa 2: Obtenha as interfaces de rede do controlador de rede para o espelhamento de origem e destino
Você pode usar os comandos de exemplo a seguir para obter as interfaces de rede do controlador de rede para o espelhamento de origem e destino.

    $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
    $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"

###<a name="step-3-create-a-serviceinsertionproperties-object-to-contain-the-port-mirroring-rules-and-the-element-which-represents-the-destination-interface"></a>Etapa 3: Criar um objeto serviceinsertionproperties para conter a porta espelhamento de regras e o elemento que representa a interface de destino
Você pode usar os comandos de exemplo a seguir para criar um objeto de serviceinsertionproperties de destino.

    $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
    $portMirror.Priority = 1

###<a name="step-4-create-a-serviceinsertionrules-object-to-contain-the-rules-that-must-be-matched-in-order-for-the-traffic-to-be-sent-to-the-appliance"></a>Etapa 4: Criar um objeto serviceinsertionrules para conter as regras que devem ser atendidas para que o tráfego seja enviado para o dispositivo

As regras definidas abaixo correspondência todo o tráfego, entrado e saído, que representa um espelho tradicional.  Se você estiver interessado em espelhamento de uma porta específica ou fonte/destinos específicos, você pode ajustar essas regras.

Você pode usar os comandos de exemplo a seguir para criar um objeto serviceinsertionproperties.

    $portmirror.ServiceInsertionRules = [Microsoft.Windows.NetworkController.ServiceInsertionRule[]]::new(1)

    $portmirror.ServiceInsertionRules[0] = [Microsoft.Windows.NetworkController.ServiceInsertionRule]::new()
    $portmirror.ServiceInsertionRules[0].ResourceId = "Rule1"
    $portmirror.ServiceInsertionRules[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionRuleProperties]::new()

    $portmirror.ServiceInsertionRules[0].Properties.Description = "Port Mirror Rule"
    $portmirror.ServiceInsertionRules[0].Properties.Protocol = "All"
    $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeStart = "0"
    $portmirror.ServiceInsertionRules[0].Properties.SourcePortRangeEnd = "65535"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeStart = "0"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationPortRangeEnd = "65535"
    $portmirror.ServiceInsertionRules[0].Properties.SourceSubnets = "*"
    $portmirror.ServiceInsertionRules[0].Properties.DestinationSubnets = "*"

###<a name="step-5-create-a-serviceinsertionelements-object-to-contain-the-network-interface-of-the-appliance-you-are-mirroring-to"></a>Etapa 5: Criar um objeto serviceinsertionelements para conter a interface de rede do aparelho para que espelhamento de
Você pode usar os comandos de exemplo a seguir para criar um objeto de serviceinsertionelements de interface de rede.

    $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

    $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
    $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
    $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

    $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
    $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
    $portmirror.ServiceInsertionElements[0].Properties.Order = 1

###<a name="step-6-add-the-service-insertion-object-in-network-controller"></a>Etapa 6: Adicionar o objeto de serviço de inserção no controlador de rede
Quando você executa esse comando, deixará de todo o tráfego para a interface de rede do dispositivo especificado na etapa anterior.

Você pode usar os comandos de exemplo a seguir para adicionar o objeto de inserção de serviço no controlador de rede.

    $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"

###<a name="step-7-update-the-network-interface-of-the-source-to-be-mirrored"></a>Etapa 7: Atualizar a interface de rede da origem ser espelhados
Você pode usar os comandos de exemplo a seguir para atualizar a interface de rede.

    $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
    $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId

Quando você concluir essas etapas, o tráfego da interface MyVM_Ethernet1 é espelhado pela interface Appliance_Ethernet1.
 
