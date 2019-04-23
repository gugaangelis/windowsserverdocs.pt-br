---
title: Controlador de rede
description: Este tópico fornece uma visão geral do controlador de rede no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-sdn
ms.topic: article
ms.assetid: 31f3fa4e-cd25-4bf3-89e9-a01a6cec7893
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 7ace628c6ae9802c0c65d360aedfac8c80ac5537
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59875677"
---
# <a name="network-controller"></a>Controlador de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Novo no Windows Server 2016, o controlador de rede fornece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede física e virtual em seu datacenter. 

Usando o Controlador de Rede, você pode automatizar a configuração da infraestrutura de rede em vez de executar a configuração manual dos dispositivos e dos serviços de rede.

> [!NOTE]
> Além deste tópico, a seguinte documentação do controlador de rede está disponível.
> - [Alta disponibilidade do controlador de rede](network-controller-high-availability.md)
> - [Instalação e requisitos de preparação para implantar o controlador de rede](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Instalar a função de servidor de controlador de rede usando o Gerenciador do servidor](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Etapas de pós-implantação para controlador de rede](post-deploy-steps-nc.md)
> - [Cmdlets de controlador de rede](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>Visão geral do controlador de rede

Controlador de rede é uma função de servidor altamente disponível e escalonável e fornece uma interface de programação de aplicativo \(API\) que permite que o controlador de rede para se comunicar com a rede e uma segunda API que permite que você se comunicar com o controlador de rede.

Você pode implantar o controlador de rede em ambientes de fora do domínio e de domínio. Em ambientes de domínio, o controlador de rede autentica os usuários e dispositivos de rede usando o Kerberos; em ambientes fora do domínio, você deve implantar certificados para autenticação.

>[!IMPORTANT]
>Não implante a função de servidor do controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor do controlador de rede em uma máquina virtual de Hyper-V \(VM\) que é instalado em um host do Hyper-V. Depois que você instalou o controlador de rede em máquinas virtuais no Hyper diferentes três\-hosts V, você deve habilitar o Hyper\-hosts V para a rede definida pelo Software \(SDN\) adicionando hosts usando o controlador de rede o comando do Windows PowerShell **New-NetworkControllerServer**. Ao fazer isso, você habilitará o balanceador de carga de Software SDN à função. Para obter mais informações, consulte [New-NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

O Controlador de Rede se comunica com os dispositivos, os serviços e os componentes de rede usando a API Southbound. Com a API Southbound, o Controlador de Rede pode descobrir dispositivos de rede, detectar configurações de serviço e reunir todas as informações que você precisa sobre a rede. Além disso, a API Southbound fornece ao Controlador de Rede um caminho para enviar informações à infraestrutura de rede, como as alterações de configuração que você fez.

A API Northbound do Controlador de Rede fornece a capacidade de reunir informações da rede por meio do Controlador de Rede a fim de usá-las para monitorar e configurar a rede.

A API Northbound do controlador de rede permite que você configurar, monitorar, solucionar problemas e implantar novos dispositivos na rede usando o Windows PowerShell, a transferência de estado representacional \(REST\) API ou um aplicativo de gerenciamento com uma interface gráfica do usuário, como o System Center Virtual Machine Manager.

>[!NOTE]
>A API Northbound do Controlador de Rede é implementada como uma interface REST.

Você pode gerenciar sua rede de datacenter com o controlador de rede usando aplicativos de gerenciamento, como o System Center Virtual Machine Manager \(SCVMM\)e o System Center Operations Manager \(SCOM\), porque o controlador de rede permite configurar, monitorar, programar e solucionar problemas de infraestrutura de rede que está sob seu controle.

Usando o Windows PowerShell, a API REST ou um aplicativo de gerenciamento, você pode usar o Controlador de Rede para gerenciar a seguinte infraestrutura de rede física e virtual:

- VMs e comutadores virtuais do Hyper-V

- Firewall do datacenter

- Serviço de acesso remoto \(RAS\) Gateways multilocatário, Gateways virtuais e pools de gateway

- Balanceadores de carga de software

Na ilustração a seguir, um administrador usa uma ferramenta de gerenciamento que interage diretamente com o Controlador de Rede. Controlador de rede fornece informações sobre a infraestrutura de rede, incluindo a infraestrutura virtual e física, a ferramenta de gerenciamento e faz as alterações de configuração de acordo com as ações do administrador ao usar a ferramenta.  

![Visão geral do controlador de rede](../../../media/Network-Controller/NetController_overview.png)  

Se você estiver implantando o controlador de rede em um ambiente de laboratório de teste, você pode executar a função de servidor do controlador de rede em uma máquina virtual Hyper-V \(VM\) que é instalado em um host do Hyper-V.

Para alta disponibilidade em datacenters maiores, você pode implantar um cluster usando três VMs que estão instaladas em três ou mais hosts do Hyper-V. Para obter mais informações, consulte [alta disponibilidade do controlador de rede](network-controller-high-availability.md).

## <a name="bkmk_features"></a>Recursos do controlador de rede

Os seguintes recursos do Controlador de Rede permitem configurar e gerenciar dispositivos e serviços de redes físicas e virtuais.  
  
-   [Gerenciamento de firewall](#bkmk_firewall)  
  
-   [Gerenciamento de Balanceador de carga de software](#bkmk_slb)  
  
-   [Gerenciamento de rede virtual](#bkmk_virtual)  
  
-   [Gerenciamento de Gateway RAS](#bkmk_gateway)

>[!IMPORTANT]
>Backup do controlador de rede e a restauração não está disponível atualmente no Windows Server 2016.
  
### <a name="bkmk_firewall"></a>Gerenciamento de firewall

Este recurso do Controlador de Rede permite configurar e gerenciar regras de Controle de Acesso para permitir/negar firewall para as suas VMs de carga de trabalho para tráfego de rede tanto no sentido leste/oeste quanto no sentido norte/sul em seu datacenter. As regras de firewall são conectadas na porta vSwitch das VMs de carga de trabalho e distribuídas pela carga de trabalho no datacenter. Usando a API Northbound, você pode definir as regras de firewall para tráfego tanto de entrada quanto de saída na VM de carga de trabalho. Você também pode configurar cada regra de firewall para registrar o tráfego que é permitido ou negado pela regra.  

Para obter mais informações, consulte [visão geral do Firewall do Datacenter](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).

### <a name="bkmk_slb"></a>Gerenciamento de Balanceador de carga de software

Este recurso do Controlador de Rede permite habilitar vários servidores para hospedar a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.  
  
Para obter mais informações, consulte [balanceamento de carga de Software &#40;SLB&#41; para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="bkmk_virtual"></a>Gerenciamento de rede virtual

Este recurso do Controlador de Rede permite implantar e configurar a Virtualização de Rede Hyper-V, incluindo o Comutador Virtual do Hyper-V e os adaptadores de rede virtuais em VMs individuais, além de armazenar e distribuir politicas de rede virtual.

O Controlador de Rede dá suporte a NVGRE (protocolo GRE de virtualização de rede) e a VXLAN (rede local virtual extensível).

### <a name="bkmk_gateway"></a>Gerenciamento de Gateway RAS

Este recurso do controlador de rede permite que você implantar, configurar e gerenciar máquinas virtuais (VMs) que são membros de um pool de Gateway de RAS, fornecendo serviços de gateway para seus locatários. Controlador de rede permite que você implante automaticamente as VMs que executam o Gateway de RAS com os seguintes recursos de gateway:

> [!NOTE]
> No System Center Virtual Machine Manager, Gateway de RAS é chamado de Gateway do Windows Server.

- Adição e remoção das VMs do gateway do cluster e especificação do nível de backup necessário.

- Conectividade de gateway VPN (rede virtual privada) site a site entre redes de locatários remotos e seu datacenter usando IPsec.

- Conectividade de gateway VPN site a site entre redes de locatários remotos e seu datacenter usando protocolo GRE.

- Capacidade de encaminhamento de camada 3.

- Border Gateway Protocol (BGP) roteamento, que lhe permite gerenciar o roteamento de tráfego de rede entre redes VM de seus locatários e seus sites remotos.

Controlador de rede pode colocar diferentes conexões de um locatário em gateways separados. Você pode usar um único IP público para todas as conexões de gateway ou têm diferentes IPs públicos para um subconjunto das conexões. Controlador de rede registra todas as configuração de gateway e alterações de estado, que podem ser usadas para fins de solução de problemas e auditoria.

Para obter mais informações sobre o BGP, consulte [Border Gateway Protocol &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Para obter mais informações sobre o Gateway de RAS, consulte [Gateway de RAS para SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Opções de implantação do controlador de rede

Para implantar o controlador de rede usando o System Center Virtual Machine Manager \(VMM\), consulte [configurar um controlador de rede SDN na malha do VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Para implantar o controlador de rede usando scripts, consulte [implantar um Software definido infraestrutura usando Scripts de rede](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implantar o controlador de rede usando o Windows PowerShell, consulte [implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
