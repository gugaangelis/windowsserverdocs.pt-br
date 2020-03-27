---
title: Controlador de rede
description: Este tópico fornece uma visão geral do controlador de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ad13e41e756f0185a748fe9e17df64c71a8754bc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80317077"
---
# <a name="network-controller"></a>Controlador de rede

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Novidade no Windows Server 2016, o controlador de rede fornece um ponto de automação centralizado e programável para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter. 

Usando o Controlador de Rede, você pode automatizar a configuração da infraestrutura de rede em vez de executar a configuração manual dos dispositivos e dos serviços de rede.

> [!NOTE]
> Além deste tópico, a documentação do controlador de rede a seguir está disponível.
> - [Alta disponibilidade do controlador de rede](network-controller-high-availability.md)
> - [Requisitos de instalação e preparação para a implantação do controlador de rede](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Implantar controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Instalar a função de servidor do Controlador de Rede usando o Gerenciador do Servidor](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Etapas pós-implantação para o controlador de rede](post-deploy-steps-nc.md)
> - [Cmdlets do controlador de rede](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="network-controller-overview"></a><a name="bkmk_overview"></a>Visão geral do controlador de rede

O controlador de rede é uma função de servidor altamente disponível e escalonável e fornece uma interface de programação de aplicativo \(\) de API que permite que o controlador de rede se comunique com a rede e uma segunda API que permite que você se comunique com o controlador de rede.

Você pode implantar o controlador de rede em ambientes de domínio e fora do domínio. Em ambientes de domínio, o controlador de rede autentica usuários e dispositivos de rede usando o Kerberos; em ambientes que não são de domínio, você deve implantar certificados para autenticação.

>[!IMPORTANT]
>Não implante a função de servidor do controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor do controlador de rede em uma máquina virtual do Hyper-V \(\) da VM que está instalada em um host do Hyper-V. Depois de instalar o controlador de rede em VMs em três hosts Hyper\-V diferentes, você deve habilitar os hosts do Hyper\-V para a rede definida pelo software \(SDN\) adicionando os hosts ao controlador de rede usando o comando **New-NetworkControllerServer**do Windows PowerShell. Ao fazer isso, você está permitindo que o software SDN Load Balancer funcione. Para obter mais informações, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

O Controlador de Rede se comunica com os dispositivos, os serviços e os componentes de rede usando a API Southbound. Com a API Southbound, o Controlador de Rede pode descobrir dispositivos de rede, detectar configurações de serviço e reunir todas as informações que você precisa sobre a rede. Além disso, a API Southbound fornece ao Controlador de Rede um caminho para enviar informações à infraestrutura de rede, como as alterações de configuração que você fez.

A API Northbound do Controlador de Rede fornece a capacidade de reunir informações da rede por meio do Controlador de Rede a fim de usá-las para monitorar e configurar a rede.

A API do controlador de rede Northbound permite que você configure, monitore, solucione problemas e implante novos dispositivos na rede usando o Windows PowerShell, a transferência de estado de reapresentação \(REST\) API ou um aplicativo de gerenciamento com uma interface gráfica do usuário, como System Center Virtual Machine Manager.

>[!NOTE]
>A API Northbound do Controlador de Rede é implementada como uma interface REST.

Você pode gerenciar sua rede de datacenter com o controlador de rede usando aplicativos de gerenciamento, como System Center Virtual Machine Manager \(SCVMM\)e System Center Operations Manager \(SCOM\), pois o controlador de rede permite configurar, monitorar, programar e solucionar problemas da infraestrutura de rede sob seu controle.

Usando o Windows PowerShell, a API REST ou um aplicativo de gerenciamento, você pode usar o Controlador de Rede para gerenciar a seguinte infraestrutura de rede física e virtual:

- VMs e comutadores virtuais do Hyper-V

- Firewall do Datacenter

- Serviço de acesso remoto \(RAS\) gateways de multilocatário, gateways virtuais e pools de gateway

- Balanceadores de carga de software

Na ilustração a seguir, um administrador usa uma ferramenta de gerenciamento que interage diretamente com o Controlador de Rede. O controlador de rede fornece informações sobre a infraestrutura de rede, incluindo infraestrutura física e virtual, para a ferramenta de gerenciamento e faz alterações de configuração de acordo com as ações do administrador ao usar a ferramenta.  

![Visão geral do controlador de rede](../../../media/Network-Controller/NetController_overview.png)  

Se você estiver implantando o controlador de rede em um ambiente de laboratório de teste, poderá executar a função de servidor do controlador de rede em uma máquina virtual do Hyper-V \(\) da VM instalada em um host do Hyper-V.

Para alta disponibilidade em data centers maiores, você pode implantar um cluster usando três VMs instaladas em três ou mais hosts Hyper-V. Para obter mais informações, consulte [alta disponibilidade do controlador de rede](network-controller-high-availability.md).

## <a name="network-controller-features"></a><a name="bkmk_features"></a>Recursos do controlador de rede

Os seguintes recursos do Controlador de Rede permitem configurar e gerenciar dispositivos e serviços de redes físicas e virtuais.  
  
-   [Gerenciamento de firewall](#bkmk_firewall)  
  
-   [Gerenciamento de Load Balancer de software](#bkmk_slb)  
  
-   [Gerenciamento de rede virtual](#bkmk_virtual)  
  
-   [Gerenciamento de gateway RAS](#bkmk_gateway)

>[!IMPORTANT]
>O backup e a restauração do controlador de rede não estão disponíveis atualmente no Windows Server 2016.
  
### <a name="firewall-management"></a><a name="bkmk_firewall"></a>Gerenciamento de firewall

Este recurso do Controlador de Rede permite configurar e gerenciar regras de Controle de Acesso para permitir/negar firewall para as suas VMs de carga de trabalho para tráfego de rede tanto no sentido leste/oeste quanto no sentido norte/sul em seu datacenter. As regras de firewall são conectadas na porta vSwitch das VMs de carga de trabalho e distribuídas pela carga de trabalho no datacenter. Usando a API Northbound, você pode definir as regras de firewall para tráfego tanto de entrada quanto de saída na VM de carga de trabalho. Você também pode configurar cada regra de firewall para registrar o tráfego que é permitido ou negado pela regra.  

Para obter mais informações, consulte [visão geral do firewall do datacenter](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).

### <a name="software-load-balancer-management"></a><a name="bkmk_slb"></a>Gerenciamento de Load Balancer de software

Este recurso do Controlador de Rede permite habilitar vários servidores para hospedar a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.  
  
Para obter mais informações, consulte [ &#40;SLB&#41; de balanceamento de carga de software para Sdn](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="virtual-network-management"></a><a name="bkmk_virtual"></a>Gerenciamento de rede virtual

Este recurso do Controlador de Rede permite implantar e configurar a Virtualização de Rede Hyper-V, incluindo o Comutador Virtual do Hyper-V e os adaptadores de rede virtuais em VMs individuais, além de armazenar e distribuir politicas de rede virtual.

O Controlador de Rede dá suporte a NVGRE (protocolo GRE de virtualização de rede) e a VXLAN (rede local virtual extensível).

### <a name="ras-gateway-management"></a><a name="bkmk_gateway"></a>Gerenciamento de gateway RAS

Esse recurso de controlador de rede permite implantar, configurar e gerenciar máquinas virtuais (VMs) que são membros de um pool de gateway de RAS, fornecendo serviços de gateway para seus locatários. O controlador de rede permite que você implante automaticamente as VMs que executam o gateway RAS com os seguintes recursos de gateway:

> [!NOTE]
> No System Center Virtual Machine Manager, o gateway RAS é chamado de gateway do Windows Server.

- Adição e remoção das VMs do gateway do cluster e especificação do nível de backup necessário.

- Conectividade de gateway VPN (rede virtual privada) site a site entre redes de locatários remotos e seu datacenter usando IPsec.

- Conectividade de gateway VPN site a site entre redes de locatários remotos e seu datacenter usando protocolo GRE.

- Capacidade de encaminhamento de camada 3.

- Roteamento de Border Gateway Protocol (BGP), que permite gerenciar o roteamento de tráfego de rede entre as redes de VM de seus locatários e seus sites remotos.

O controlador de rede pode fazer conexões diferentes de um locatário em gateways separados. Você pode usar um único IP público para todas as conexões de gateway ou ter IPs públicos diferentes para um subconjunto das conexões. O controlador de rede registra todas as configurações de gateway e as alterações de estado, que podem ser usadas para fins de auditoria e solução de problemas.

Para obter mais informações sobre o BGP [, &#40;consulte&#41;Border Gateway Protocol BGP](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Para obter mais informações sobre o gateway RAS, consulte [Gateway de RAS para Sdn](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Opções de implantação do controlador de rede

Para implantar o controlador de rede usando System Center Virtual Machine Manager \(\)do VMM, consulte [configurar um controlador de rede Sdn na malha do VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Para implantar o controlador de rede usando scripts, consulte [implantar uma infraestrutura de rede definida pelo software usando scripts](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implantar o controlador de rede usando o Windows PowerShell, consulte [implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
