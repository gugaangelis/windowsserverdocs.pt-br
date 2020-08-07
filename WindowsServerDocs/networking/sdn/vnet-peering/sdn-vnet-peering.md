---
title: Emparelhamento de rede virtual
manager: grcusanz
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/08/2018
ms.openlocfilehash: 21008268f14435852c7de78ce826bc380f9017a1
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87955813"
---
# <a name="virtual-network-peering"></a>Emparelhamento de rede virtual

>Aplica-se a: Windows Server

O emparelhamento de rede virtual permite que você conecte duas redes virtuais diretamente. Uma vez emparelhadas, para fins de conectividade, as redes virtuais aparecem como uma.

Os benefícios do uso do emparelhamento de rede virtual incluem:

-   O tráfego entre as máquinas virtuais nas redes virtuais emparelhadas é roteado por meio da infraestrutura de backbone somente por meio de endereços IP *privados* . A comunicação entre as redes virtuais não requer Internet ou gateways públicos.

-   Baixa latência, conexão com largura de banda alta entre os recursos em redes virtuais diferentes.

-   A capacidade de recursos em uma rede virtual se comunicarem com recursos em uma rede virtual diferente.

-   Nenhum tempo de inatividade para recursos em nenhuma rede virtual ao criar o emparelhamento.

## <a name="requirements-and-constraints"></a>Requisitos e restrições

O emparelhamento de rede virtual tem alguns requisitos e restrições:

- As redes virtuais emparelhadas devem:

  -   Ter espaços de endereço IP não sobrepostos

  -   Ser gerenciado pelo mesmo controlador de rede

- Depois de emparelhar uma rede virtual com outra rede virtual, você não poderá adicionar ou excluir intervalos de endereços no espaço de endereço.

  >[!TIP]
  >Se você precisar adicionar intervalos de endereços:<ol><li>Remova o emparelhamento.</li><li>Adicione o espaço de endereço.</li><li>Adicione o emparelhamento novamente.</li></ol>

- Como o emparelhamento de rede virtual está entre duas redes virtuais, não há nenhuma relação transitiva derivada entre emparelhamentos. Por exemplo, se você emparelhar virtualNetworkA com virtualNetworkB e virtualNetworkB com virtualNetworkC, o virtualNetworkA não será emparelhado com virtualNetworkC.

## <a name="connectivity"></a>Conectividade

Depois de emparelhar as redes virtuais, os recursos em qualquer rede virtual podem se conectar diretamente com os recursos na rede virtual emparelhada.

-   A latência de rede entre as máquinas virtuais nas redes virtuais emparelhadas é igual à latência em uma única rede virtual.

-   A taxa de transferência de rede é baseada na largura de banda permitida para a máquina virtual. Não existe restrição adicional quanto à largura de banda no emparelhamento.

-   O tráfego entre máquinas virtuais em redes virtuais emparelhadas é roteado diretamente por meio da infraestrutura de backbone, não por meio de um gateway ou pela Internet pública.

-   As máquinas virtuais em uma rede virtual podem acessar o balanceador de carga interno na rede virtual emparelhada.

Você pode aplicar listas de controle de acesso (ACLs) em qualquer rede virtual para bloquear o acesso a outras redes virtuais ou sub-redes, se desejado. Se você abrir a conectividade completa entre redes virtuais emparelhadas (que é a opção padrão), poderá aplicar ACLs a sub-redes ou máquinas virtuais específicas para bloquear ou negar acesso específico. Para saber mais sobre ACLs, confira [usar ACLs (listas de controle de acesso) para gerenciar o fluxo de tráfego de rede do datacenter](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-acls-for-traffic-flow).

## <a name="service-chaining"></a>Encadeamento de serviços

Você pode configurar rotas definidas pelo usuário que apontem para máquinas virtuais em redes virtuais emparelhadas como o endereço IP do próximo salto, para habilitar o encadeamento de serviços. O encadeamento de serviços permite direcionar o tráfego de uma rede virtual para uma solução de virtualização, em uma rede virtual emparelhada, por meio de rotas definidas pelo usuário.

Você pode implantar redes Hub e spoke, em que a rede virtual do Hub pode hospedar componentes de infraestrutura, como uma solução de virtualização de rede. Todas as redes virtuais spoke emparelhadas com a rede virtual do Hub. O tráfego pode fluir por meio de dispositivos de rede virtual na rede virtual do Hub.

O emparelhamento de rede virtual permite que o próximo salto em uma rota definida pelo usuário seja o endereço IP de uma máquina virtual na rede virtual emparelhada. Para saber mais sobre as rotas definidas pelo usuário, confira [usar dispositivos de rede virtual em uma rede virtual](https://docs.microsoft.com/windows-server/networking/sdn/manage/use-network-virtual-appliances-on-a-vn).

## <a name="gateways-and-on-premises-connectivity"></a>Gateways e conectividade local

Cada rede virtual, independentemente de estar emparelhada com outra rede virtual, ainda pode ter seu próprio gateway para se conectar a uma rede local. Ao emparelhar redes virtuais, você também pode configurar o gateway na rede virtual emparelhada como um ponto de trânsito para uma rede local. Nesse caso, a rede virtual que usa um gateway remoto não pode ter seu próprio gateway. Uma rede virtual pode ter apenas um gateway que pode ser um gateway local ou remoto (na rede virtual emparelhada).

## <a name="monitor"></a>Monitoramento

Ao emparelhar duas redes virtuais, você deve configurar um emparelhamento para cada rede virtual no emparelhamento.

Você pode monitorar o status da conexão de emparelhamento, que pode estar em um dos seguintes Estados:

-   **Iniciado em:** Mostrado quando você cria o emparelhamento da primeira rede virtual para a segunda rede virtual.

-   **Conectado:** Mostrado depois de criar o emparelhamento da segunda rede virtual para a primeira rede virtual. O estado de emparelhamento da primeira rede virtual muda de Iniciado para Conectado. Ambos os pares de rede virtual devem ter o estado conectado antes de estabelecer um emparelhamento de rede virtual com êxito.

-   **Desconectado:** Mostrado se uma rede virtual se desconectar de outra rede virtual.

[infográfico dos Estados]

## <a name="next-steps"></a>Próximas etapas
[Configurar o emparelhamento de rede virtual](sdn-configure-vnet-peering.md): neste procedimento, você usa o Windows PowerShell para localizar a rede lógica do provedor de HNV para criar duas redes virtuais, cada uma com uma sub-rede. Você também configura o emparelhamento entre as duas redes virtuais.

