---
title: Usar dispositivos virtuais de rede em uma rede virtual
description: Neste tópico, você aprenderá a implantar dispositivos de rede virtual em redes virtuais de locatário. Você pode adicionar soluções de virtualização de rede a redes que executam funções de espelhamento de porta e roteamento definidas pelo usuário.
manager: grcusanz
ms.topic: article
ms.prod: windows-server
ms.technology: networking-sdn
ms.assetid: 3c361575-1050-46f4-ac94-fa42102f83c1
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/30/2018
ms.openlocfilehash: 5d8ac7256e9c7e59c7df260bea5d5a8f0fb6b42b
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854469"
---
# <a name="use-network-virtual-appliances-on-a-virtual-network"></a>Usar dispositivos virtuais de rede em uma rede virtual

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, você aprenderá a implantar dispositivos de rede virtual em redes virtuais de locatário. Você pode adicionar soluções de virtualização de rede a redes que executam funções de espelhamento de porta e roteamento definidas pelo usuário.

## <a name="types-of-network-virtual-appliances"></a>Tipos de dispositivos de rede virtual

Você pode usar um dos dois tipos de dispositivos virtuais:

1. **Roteamento definido pelo usuário** – substitui os roteadores distribuídos na rede virtual pelos recursos de roteamento do dispositivo virtual.  Com o roteamento definido pelo usuário, a solução de virtualização é usada como um roteador entre as sub-redes virtuais na rede virtual.

2. **Espelhamento de porta** – todo o tráfego de rede que está entrando ou saindo da porta monitorada é duplicado e enviado a um dispositivo virtual para análise. 


## <a name="deploying-a-network-virtual-appliance"></a>Implantando uma solução de virtualização de rede

Para implantar uma solução de virtualização de rede, você deve primeiro criar uma VM que contenha o dispositivo e, em seguida, conectar a VM às sub-redes de rede virtual apropriadas. Para obter mais detalhes, consulte [criar uma VM de locatário e conectar-se a uma rede virtual de locatário ou VLAN](Create-a-Tenant-VM.md).

Alguns dispositivos exigem vários adaptadores de rede virtual. Normalmente, um adaptador de rede dedicado ao gerenciamento de dispositivo, enquanto adaptadores adicionais processam o tráfego.  Se seu dispositivo exigir vários adaptadores de rede, você deverá criar cada interface de rede no controlador de rede. Você também deve atribuir uma ID de interface em cada host para cada um dos adaptadores adicionais que estão em sub-redes virtuais diferentes.

Depois de implantar a solução de virtualização de rede, você pode usar o dispositivo para roteamento definido, portamento de espelhamento ou ambos. 


## <a name="example-user-defined-routing"></a>Exemplo: roteamento definido pelo usuário

Para a maioria dos ambientes, você só precisa das rotas do sistema já definidas pelo roteador distribuído da rede virtual. No entanto, talvez seja necessário criar uma tabela de roteamento e adicionar uma ou mais rotas em casos específicos, como:

- Forçar o túnel para a Internet por meio de sua rede local.
- Uso de dispositivos virtuais em seu ambiente.

Para esses cenários, você deve criar uma tabela de roteamento e adicionar rotas definidas pelo usuário à tabela. Você pode ter várias tabelas de roteamento e associar a mesma tabela de roteamento a uma ou mais sub-redes. Você só pode associar cada sub-rede a uma única tabela de roteamento. Todas as VMs em uma sub-rede usam a tabela de roteamento associada à sub-rede.

As sub-redes dependem de rotas do sistema até que uma tabela de roteamento seja associada à sub-rede. Após a existência de uma associação, o roteamento é feito com base na maior correspondência de prefixo (LPM) entre as rotas definidas pelo usuário e as rotas do sistema. Se houver mais de uma rota com a mesma correspondência de LPM, a rota definida pelo usuário será selecionada primeiro-antes da rota do sistema.
 
**Procedure**

1. Crie as propriedades da tabela de rotas, que contém todas as rotas definidas pelo usuário.<p>As rotas do sistema ainda se aplicam de acordo com as regras definidas acima.

   ```PowerShell
    $routetableproperties = new-object Microsoft.Windows.NetworkController.RouteTableProperties
   ```

2. Adicione uma rota às propriedades da tabela de roteamento.<p>Qualquer rota destinada à sub-rede 12.0.0.0/8 é roteada para a solução de virtualização em 192.168.1.10. O dispositivo deve ter um adaptador de rede virtual conectado à rede virtual com esse IP atribuído a uma interface de rede.

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

4. Aplique a tabela de roteamento à sub-rede virtual.<p>Quando você aplica a tabela de rotas à sub-rede virtual, a primeira sub-rede virtual na Tenant1_Vnet1 rede usa a tabela de rotas. Você pode atribuir a tabela de rotas para o máximo de sub-redes na rede virtual que desejar.

   ```PowerShell
    $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
    $vnet.properties.subnets[0].properties.RouteTable = $routetable
    new-networkcontrollervirtualnetwork -connectionuri $uri -properties $vnet.properties -resourceId $vnet.resourceid
   ```

Assim que você aplicar a tabela de roteamento à rede virtual, o tráfego será encaminhado para o dispositivo virtual. Você deve configurar a tabela de roteamento no dispositivo virtual para encaminhar o tráfego de uma maneira apropriada para o seu ambiente.

## <a name="example-port-mirroring"></a>Exemplo: espelhamento de porta

Neste exemplo, você configura o tráfego para MyVM_Ethernet1 espelhar Appliance_Ethernet1.  Presumimos que você tenha implantado duas VMs, uma como o dispositivo e a outra como a VM a ser monitorada com o espelhamento. 

O dispositivo deve ter uma segunda interface de rede para gerenciamento. Depois de habilitar o espelhamento como um destino no Appliciance_Ethernet1, ele não recebe mais o tráfego destinado à interface IP configurada lá.


**Procedure**

1. Obtenha a rede virtual na qual suas VMs estão localizadas.

   ```PowerShell
   $vnet = Get-NetworkControllerVirtualNetwork -ConnectionUri $uri -ResourceId "Tenant1_VNet1"
   ```

2. Obtenha as interfaces de rede do controlador de rede para a origem e o destino do espelhamento.

   ```PowerShell
   $dstNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "Appliance_Ethernet1"
   $srcNic = get-networkcontrollernetworkinterface -ConnectionUri $uri -ResourceId "MyVM_Ethernet1"
   ```

3. Crie um objeto iminsertproperties para conter as regras de espelhamento de porta e o elemento que representa a interface de destino.

   ```PowerShell
   $portmirror = [Microsoft.Windows.NetworkController.ServiceInsertionProperties]::new()
   $portMirror.Priority = 1
   ```

4. Crie um objeto serviceinsertionrules para conter as regras que devem ser correspondidas para que o tráfego seja enviado para o dispositivo.<p>As regras definidas abaixo correspondem a todo o tráfego, tanto de entrada quanto de saída, que representa um espelho tradicional.  Você pode ajustar essas regras se estiver interessado em espelhar uma porta específica ou origem/destinos específicos.

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

5. Crie um objeto serviceinsertionelements para conter a interface de rede do dispositivo espelhado.

   ```PowerShell
   $portmirror.ServiceInsertionElements = [Microsoft.Windows.NetworkController.ServiceInsertionElement[]]::new(1)

   $portmirror.ServiceInsertionElements[0] = [Microsoft.Windows.NetworkController.ServiceInsertionElement]::new()
   $portmirror.ServiceInsertionElements[0].ResourceId = "Element1"
   $portmirror.ServiceInsertionElements[0].Properties = [Microsoft.Windows.NetworkController.ServiceInsertionElementProperties]::new()

   $portmirror.ServiceInsertionElements[0].Properties.Description = "Port Mirror Element"
   $portmirror.ServiceInsertionElements[0].Properties.NetworkInterface = $dstNic
   $portmirror.ServiceInsertionElements[0].Properties.Order = 1
   ```

6. Adicione o objeto de inserção de serviço no controlador de rede.<p>Quando você emite esse comando, todo o tráfego para a interface de rede do dispositivo especificado na etapa anterior é interrompido.

   ```PowerShell
   $portMirror = New-NetworkControllerServiceInsertion -ConnectionUri $uri -Properties $portmirror -ResourceId "MirrorAll"
   ```

7. Atualize a interface de rede da origem a ser espelhada.

   ```PowerShell
   $srcNic.Properties.IpConfigurations[0].Properties.ServiceInsertion = $portMirror
   $srcNic = New-NetworkControllerNetworkInterface -ConnectionUri $uri  -Properties $srcNic.Properties -ResourceId $srcNic.ResourceId
   ```

Depois de concluir essas etapas, a interface de Appliance_Ethernet1 espelha o tráfego da interface MyVM_Ethernet1.
 
---