---
title: Emparelhamento de rede virtual
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/08/2018
ms.openlocfilehash: 58596387d79f3f212a472f00c2785bacc278e855
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821907"
---
# <a name="virtual-network-peering"></a>Emparelhamento de rede virtual

>Aplica-se a: Windows Server

Emparelhamento de rede virtual permite que você conecte duas redes virtuais perfeitamente. Uma vez emparelhadas, para fins de conectividade, as redes virtuais aparecerão como um. 

Os benefícios do uso do emparelhamento de rede virtual incluem:

-   O tráfego entre máquinas virtuais em redes virtuais emparelhadas é roteado por meio da infraestrutura de backbone através de *privada* somente por endereços IP. A comunicação entre as redes virtuais não requer público à Internet ou gateways.

-   Uma conexão de baixa latência e largura de banda alta entre os recursos em redes virtuais diferentes.

-   A capacidade de recursos em uma rede virtual para se comunicar com os recursos em uma rede virtual diferente.

-   Sem tempo de inatividade para recursos em uma rede virtual ao criar o emparelhamento.

## <a name="requirements-and-constraints"></a>Requisitos e restrições

Emparelhamento de rede virtual tem alguns requisitos e restrições:

-   Redes virtuais emparelhadas devem:

    -   Ter espaços de endereço IP não sobrepostos

    -   Ser gerenciado pelo mesmo controlador de rede

-   Depois que você emparelhar uma rede virtual com outra rede virtual, é possível adicionar ou excluir intervalos de endereços no espaço de endereço.

   >[!TIP]
   >Se você precisar adicionar intervalos de endereços:<ol><li>Remova o emparelhamento.</li><li>Adicione o espaço de endereço.</li><li>Adicione o emparelhamento.</li></ol>

-   Como o emparelhamento de rede virtual é entre duas redes virtuais, não há nenhuma relação transitiva derivada entre emparelhamentos. Por exemplo, se você emparelhar virtualNetworkA com virtualNetworkB e virtualNetworkB com virtualNetworkC, em seguida, virtualNetworkA não obter emparelhada com virtualNetworkC.

    [imagem aqui]

## <a name="connectivity"></a>Conectividade

Depois que você emparelhar redes virtuais, recursos em uma rede virtual podem se conectar diretamente com os recursos na rede virtual emparelhada.

-   Latência de rede entre máquinas virtuais em redes virtuais emparelhadas é o mesmo que a latência de uma única rede virtual.

-   Taxa de transferência da rede baseia-se na largura de banda permitida para a máquina virtual. Não há restrição adicional de largura de banda no emparelhamento.

-   O tráfego entre máquinas virtuais em redes virtuais emparelhadas é roteado diretamente pela infraestrutura de backbone, não por meio de um gateway ou pela Internet pública.

-   Máquinas virtuais em uma rede virtual pode acessar o balanceador de carga interno na rede virtual emparelhada.

Você pode aplicar as listas de controle de acesso (ACLs) em qualquer rede virtual para bloquear o acesso a outras redes virtuais ou sub-redes se desejado. Se você abrir conectividade total entre redes virtuais emparelhadas (que é a opção padrão), você pode aplicar ACLs para sub-redes específicas ou máquinas virtuais para bloquear ou negar o acesso específico. Para saber mais sobre ACLs, consulte [uso Access Control Lists (ACLs) para gerenciar o data center rede tráfego fluir](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

## <a name="service-chaining"></a>Encadeamento de serviços

Você pode configurar rotas definidas pelo usuário que apontam para máquinas virtuais em redes virtuais emparelhadas como o endereço IP de próximo salto, para habilitar o encadeamento de serviço. Encadeamento de serviços permite que você direcione o tráfego de uma rede virtual para um dispositivo virtual, em uma rede virtual emparelhada através de rotas definidas pelo usuário.

Você pode implantar redes de hub e spoke, nos quais a rede virtual hub pode hospedar componentes de infraestrutura como um dispositivo de rede virtual. Todas as redes virtuais de spoke emparelhar com a rede virtual do hub. Tráfego pode fluir por dispositivos de rede virtual na rede virtual do hub.

Emparelhamento de rede virtual permite que o próximo salto em uma rota definida pelo usuário para ser o endereço IP de uma máquina virtual na rede virtual emparelhada. Para saber mais sobre rotas definidas pelo usuário, consulte [usar soluções de virtualização de rede em uma rede Virtual](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn).

## <a name="gateways-and-on-premises-connectivity"></a>Conectividade de gateways e local

Cada rede virtual, independentemente de se emparelhada com outra rede virtual, ainda pode ter seu próprio gateway para se conectar a uma rede local. Ao emparelhar redes virtuais, você também pode configurar o gateway na rede virtual emparelhada como um ponto de trânsito para uma rede local. Nesse caso, a rede virtual que usa um gateway remoto não pode ter seu próprio gateway. Uma rede virtual pode ter apenas um gateway pode ser um gateway local ou remoto (na rede virtual emparelhada).

## <a name="monitor"></a>Monitorar

Ao emparelhar duas redes virtuais, você deve configurar um emparelhamento para cada rede virtual no emparelhamento.

Você pode monitorar o status da sua conexão de emparelhamento, que pode estar em um dos seguintes estados:

-   **Iniciado:** Mostrado quando você cria o emparelhamento da primeira rede virtual para a segunda rede virtual.

-   **Conectado:** Mostrado depois de criar o emparelhamento a partir da segunda rede virtual para a primeira rede virtual. O estado de emparelhamento para a primeira rede virtual é alterado de iniciado para conectado. Ambos os pontos de rede virtual devem ter o estado do conectado antes de estabelecer uma rede virtual emparelhamento com êxito.

-   **Desconectado:** Mostra se uma rede virtual se desconecta a outra rede virtual.

[infográfico dos Estados]

## <a name="next-steps"></a>Próximas etapas
[Configurar o emparelhamento de rede virtual](sdn-configure-vnet-peering.md): Neste procedimento, você usar o Windows PowerShell para localizar a HNV rede lógica do provedor para criar duas redes virtuais, cada um com uma sub-rede. Você também pode configurar o emparelhamento entre duas redes virtuais.

