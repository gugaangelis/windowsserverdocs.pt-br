---
title: Usar dispositivos virtuais de rede em uma rede virtual
description: Neste tópico, você aprenderá como implantar soluções de virtualização de rede em redes virtuais de locatário. Você pode adicionar dispositivos de rede virtual para redes que executam o roteamento definido pelo usuário e funções de espelhamento de porta.
manager: dougkim
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
ms.date: 08/30/2018
ms.openlocfilehash: e715a782651a5b9867f3b45251fd6ea6e4a9e4f7
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847367"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Usar dispositivos virtuais de rede em uma rede virtual

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você aprenderá como implantar soluções de virtualização de rede em redes virtuais de locatário. Você pode adicionar dispositivos de rede virtual para redes que executam o roteamento definido pelo usuário e funções de espelhamento de porta.

## <a name="types-of-network-virtual-appliances"></a>Tipos de dispositivos de rede virtual

Você pode usar um dos dois tipos de soluções de virtualização:

1. **Roteamento definido pelo usuário** -substitui distribuídos roteadores na rede virtual com os recursos de roteamento do dispositivo virtual.  Com o roteamento definido pelo usuário, a solução de virtualização é usada como um roteador entre as sub-redes virtuais na rede virtual.

2. **Espelhamento de porta** - todos tráfego de rede que está entrando ou deixar a porta monitorada é duplicado e enviada para um dispositivo virtual para análise. 


## <a name="deploying-a-network-virtual-appliance"></a>Implantando uma solução de virtualização de rede

Para implantar uma solução de virtualização de rede, você deve primeiro criar uma VM que contém o dispositivo e, em seguida, conecte-se a VM para as sub-redes da rede virtual apropriada. Para obter mais detalhes, consulte [criar uma VM de locatário e conectar-se a uma rede Virtual do locatário ou VLAN](Create-a-Tenant-VM.md).

Algumas soluções exigem vários adaptadores de rede virtual. Normalmente, um adaptador de rede dedicado para o gerenciamento de dispositivo, enquanto outros adaptadores processam o tráfego.  Se seu dispositivo requer vários adaptadores de rede, você deve criar cada interface de rede no controlador de rede. Você também deve atribuir uma ID de interface em cada host para cada um dos adaptadores adicionais que estão em diferentes sub-redes virtuais.

Depois de implantar a solução de virtualização de rede, você pode usar o dispositivo de roteamento definidas, portabilidade de espelhamento, ou ambos. 


## <a name="example-user-defined-routing"></a>Exemplo: Roteamento definido pelo usuário

Para a maioria dos ambientes, você precisa apenas as rotas de sistema já definidas pelo roteador de distribuído da rede virtual. No entanto, talvez você precise criar uma tabela de roteamento e adicionar uma ou mais rotas em casos específicos, como:

- Túnel à força para a Internet através de sua rede local.
- Uso de dispositivos virtuais em seu ambiente.

Para esses cenários, você deve criar uma tabela de roteamento e adicionar as rotas definidas pelo usuário à tabela. Você pode ter várias tabelas de roteamentos, e você pode associar a mesma tabela de roteamento para uma ou mais sub-redes. Você só pode associar a cada sub-rede para uma única tabela de roteamento. Todas as VMs em uma sub-rede usam a tabela de roteamento associada à sub-rede.

As sub-redes contam com rotas de sistema até que uma tabela de roteamento obtém associada à sub-rede. Depois que houver uma associação, o roteamento é feito com base na correspondência de prefixo mais longo (LPM) entre as rotas definidas pelo usuário e as rotas do sistema. Se houver mais de uma rota com a mesma correspondência LPM, em seguida, a rota de definida pelo usuário está selecionada primeiro - antes da rota do sistema.
 
**Procedimento:**

1. Crie a rota as propriedades da tabela que contém todas as rotas definidas pelo usuário.<p>Rotas de sistema ainda se aplicam acordo com as regras definidas anteriormente.

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. Adicione uma rota para as propriedades da tabela de roteamento.<p>Nenhuma rota de destino para a sub-rede 12.0.0.0/8 seja roteada para a solução de virtualização 192.168.1.10. O dispositivo deve ter um adaptador de rede virtual conectado à rede virtual com esse IP atribuído a um adaptador de rede.

   ```PowerShell
    $route = new-object Microsoft.Windows.NetworkController.Route
    $route.ResourceID = "0_0_0_0_0"
    $route.properties = new-object Microsoft.Windows.NetworkController.RouteProperties
    $route.properties.AddressPrefix = "0.0.0.0/0"
    $route.properties.nextHopType = "VirtualAppliance"
    $route.properties.nextHopIpAddress = "192.168.1.10"
    $routetableproperties.routes += $route
   ```
   >[!TIP]
   >Se você quiser adicionar mais rotas, repita essa etapa para cada rota que você deseja definir.

3. Adicione a tabela de roteamento ao controlador de rede.

   ```PowerShell
    $routetable = New-NetworkControllerRouteTable -ConnectionUri $uri -ResourceId "Route1" -Properties $routetableproperties
   ```

4. Aplica a tabela de roteamento para a sub-rede virtual.<p>Quando você aplica a tabela de rotas à sub-rede virtual, a primeira sub-rede virtual na rede Tenant1_Vnet1 usa a tabela de rotas. Você pode atribuir a tabela de rotas para o mesmo número de sub-redes na rede virtual, como você deseja.

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

Assim que você aplicar a tabela de roteamento na rede virtual, o tráfego é encaminhado para a solução de virtualização. Você deve configurar a tabela de roteamento em que o dispositivo virtual para encaminhar o tráfego, de forma que seja apropriado para seu ambiente.

## <a name="example-port-mirroring"></a>Exemplo: Espelhamento de porta

Neste exemplo, você configura o tráfego para MyVM_Ethernet1 Appliance_Ethernet1 de espelho.  Vamos supor que você implantou duas VMs, como o dispositivo e a outra VM para monitorar com o espelhamento. 

O dispositivo deve ter um segundo adaptador de rede para o gerenciamento. Depois de habilitar o espelhamento como um destino na Appliciance_Ethernet1, ele não receba mais o tráfego destinado para a interface IP configurada lá.


**Procedimento:**

1. Obtenha a rede virtual em que suas VMs estão localizadas.

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. Obtenha as interfaces de rede do controlador de rede para o espelhamento de origem e destino.

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. Crie um objeto serviceinsertionproperties para conter a regras e o elemento que representa a interface de destino de espelhamento de porta.

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. Crie um objeto serviceinsertionrules para conter as regras que devem ser atendidas para que o tráfego seja enviado para o dispositivo.<p>As regras definidas abaixo correspondência todo o tráfego, entrado e saído, que representa um espelho tradicional.  Você pode ajustar essas regras, se você estiver interessado em uma porta específica ou origem/destinos específicos de espelhamento.

   ```PowerShell
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
   ```

5. Crie um objeto serviceinsertionelements para conter o adaptador de rede do dispositivo espelhado.

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. Adicione o objeto de inserção de serviço no controlador de rede.<p>Quando você emitir esse comando, todo o tráfego para o dispositivo de rede interface especificada nas paradas de etapa anterior.

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. Atualize a interface de rede da fonte a ser espelhado.

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

Depois de concluir essas etapas, a interface Appliance_Ethernet1 espelha o tráfego da interface MyVM_Ethernet1.
 
---