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
ms.openlocfilehash: acb9abbd716e9930fb01431e7004abb72a7da10c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-controller"></a>Controlador de rede

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Novo no Windows Server 2016, controlador de rede oferece um ponto centralizado e programável de automação para gerenciar, configurar, monitorar e solucionar problemas de infraestrutura de rede físicos e virtuais em seu datacenter. 

Usando o controlador de rede, você pode automatizar a configuração da infraestrutura de rede em vez de executar a configuração manual dos dispositivos de rede e serviços.

> [!NOTE]
> Além de neste tópico, a seguinte documentação de controlador de rede está disponível.
> - [Alta disponibilidade do controlador de rede](network-controller-high-availability.md)
> - [Instalação e requisitos de preparação para a implantação de controlador de rede](../../plan/Installation-and-Preparation-Requirements-for-Deploying-Network-Controller.md)  
> - [Implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)  
> - [Instalar a função de servidor de controlador de rede usando o Gerenciador do servidor](Install-the-Network-Controller-server-role-using-Server-Manager.md)
> - [Pós-Post-Deployment Etapas para o controlador de rede](post-deploy-steps-nc.md)
> - [Cmdlets do controlador de rede](https://technet.microsoft.com/library/mt576401.aspx) 

## <a name="bkmk_overview"></a>Visão geral de controlador de rede

Controlador de rede é uma função de servidor altamente disponível e escalonável e fornece um aplicativo programação da interface \(API\) que permite que o controlador de rede para se comunicar com a rede e uma segunda API que permite que você se comunique com o controlador de rede.

Você pode implantar o controlador de rede no domínio e ambientes de fora do domínio. Em ambientes de domínio, o controlador de rede autentica usuários e dispositivos de rede usando Kerberos; em ambientes de não do domínio, você deve implantar certificados para autenticação.

>[!IMPORTANT]
>Não implante a função de servidor de controlador de rede em hosts físicos. Para implantar o controlador de rede, você deve instalar a função de servidor de controlador de rede em uma máquina virtual de Hyper-V \(VM\) que é instalada em um host do Hyper-V. Depois que você instalou o controlador de rede em VMs em três diferentes hosts de Hyper\-V, você deve habilitar os hosts Hyper\-V para Software de rede definidos \(SDN\) adicionando os hosts de controlador de rede usando o comando do Windows PowerShell **nova NetworkControllerServer**. Fazendo isso, você está ativando o balanceador de carga de Software SDN à função. Para obter mais informações, consulte [nova NetworkControllerServer](https://technet.microsoft.com/itpro/powershell/windows/network-controller/new-networkcontrollerserver).

Controlador de rede se comunica com dispositivos de rede, serviços e componentes usando a API Southbound. Com a API Southbound, controlador de rede pode descobrir dispositivos de rede, detectar configurações de serviço e coletar todas as informações que você precisa sobre a rede. Além disso, a API Southbound oferece controlador de rede com um caminho para enviar informações para a infraestrutura de rede, como alterações de configuração que você fez.

A API em sentido norte do controlador de rede fornece a capacidade de coletar informações de rede do controlador de rede e usá-lo para monitorar e configurar a rede.

A API em sentido norte do controlador de rede permite que você configurar, monitorar, solucionar problemas e implantar novos dispositivos na rede usando o Windows PowerShell, a API de transferência de estado representacional \(REST\) ou um aplicativo de gerenciamento com uma interface gráfica do usuário, como o System Center Virtual Machine Manager.

>[!NOTE]
>A API em sentido norte do controlador de rede é implementada como uma interface REST.

Você pode gerenciar sua rede datacenter com o controlador de rede usando aplicativos de gerenciamento, como o System Center Virtual Machine Manager \(SCVMM\) e System Center Operations Manager \(SCOM\), porque o controlador de rede permite que você configurar, monitorar, programa e solucionar problemas da infraestrutura de rede que está sob seu controle.

Usando o Windows PowerShell, a API REST ou um aplicativo de gerenciamento, você pode usar o controlador de rede para gerenciar a infraestrutura de rede físicos e virtuais a seguir:

- VMs do Hyper-V e comutadores virtuais

- Datacenter Firewall

- Gateways de Multitenant \(RAS\) de serviço de acesso remoto, Gateways virtuais e pools de gateway

- Balanceadores de carga de software

A ilustração a seguir, o administrador usa uma ferramenta de gerenciamento que interage diretamente com o controlador de rede. Controlador de rede fornece informações sobre a infraestrutura de rede, incluindo infraestrutura física e virtual, para a ferramenta de gerenciamento e faz com que as alterações de configuração de acordo com as ações do administrador ao usar a ferramenta.  

![Visão geral de controlador de rede](../../../media/Network-Controller/NetController_overview.png)  

Se você estiver implantando o controlador de rede em um ambiente de laboratório de teste, você pode executar a função de servidor de controlador de rede em uma máquina virtual de Hyper-V \(VM\) que é instalada em um host do Hyper-V.

Para alta disponibilidade em datacenters maiores, você pode implantar um cluster usando três VMs que estão instaladas em três ou mais hosts Hyper-V. Para obter mais informações, consulte [rede controlador alta disponibilidade](network-controller-high-availability.md).

## <a name="bkmk_features"></a>Recursos de controlador de rede

Os seguintes recursos de controlador de rede permitem que você configure e gerencie virtual e dispositivos físicos de rede e serviços.  
  
-   [Gerenciamento de firewall](#bkmk_firewall)  
  
-   [Gerenciamento de Balanceador de carga de software](#bkmk_slb)  
  
-   [Gerenciamento de rede virtual](#bkmk_virtual)  
  
-   [Gerenciamento de Gateway RAS](#bkmk_gateway)

>[!IMPORTANT]
>Rede controlador de Backup e restauração não está disponível atualmente no Windows Server 2016.
  
### <a name="bkmk_firewall"></a>Gerenciamento de firewall

Esse recurso de controlador de rede permite que você configure e gerencie Permitir/Negar regras de controle de acesso de firewall para sua carga de trabalho VMs para Leste/Oeste e tráfego de rede do Norte/South em seu datacenter. As regras de firewall são ativadas na porta vSwitch de carga de trabalho VMs e, portanto eles são distribuídos em sua carga de trabalho no datacenter. Usando a API em sentido norte, você pode definir as regras de firewall para o tráfego de entrada e saída na carga de trabalho VM. Você também pode configurar cada regra de firewall para registrar o tráfego que foi permitido ou negado pela regra.  

Para obter mais informações, consulte [visão geral de Firewall de Datacenter](../../../sdn/technologies/network-function-virtualization/Datacenter-Firewall-Overview.md).

### <a name="bkmk_slb"></a>Gerenciamento de Balanceador de carga de software

Esse recurso de controlador de rede permite que você permitir que vários servidores hospedar a mesma carga de trabalho, fornecendo alta disponibilidade e escalabilidade.  
  
Para obter mais informações, consulte [balanceamento de carga de Software & #40; SLB & #41; para SDN](../../../sdn/technologies/network-function-virtualization/Software-Load-Balancing--SLB--for-SDN.md).  
  
### <a name="bkmk_virtual"></a>Gerenciamento de rede virtual

Esse recurso de controlador de rede permite que você implantar e configurar a virtualização de rede do Hyper-V, incluindo o comutador Virtual Hyper-V e adaptadores de rede virtual em VMs individuais, bem como armazenar e distribuir políticas de rede virtual.

Controlador de rede oferece suporte a rede virtualização genérico de roteamento de encapsulamento (NVGRE) e Virtual Extensible rede Local (VXLAN).

### <a name="bkmk_gateway"></a>Gerenciamento de Gateway RAS

Esse recurso de controlador de rede permite que você implantar, configurar e gerenciar VMs (máquinas virtuais) que são membros de um pool de Gateway RAS, fornecendo serviços de gateway para seu locatários. Controlador de rede permite que você implante automaticamente VMs executando Gateway RAS com os seguintes recursos de gateway:

> [!NOTE]
> No System Center Virtual Machine Manager, o Gateway RAS é nomeado Gateway de servidor do Windows.

- Adicione e remova o gateway VMs do cluster e especificar o nível de backup necessário.

- Conectividade de gateway-to-site rede virtual privada (VPN) entre redes de locatário remoto e seu datacenter usando IPsec.

- Conectividade de gateway-to-site VPN entre redes de locatário remoto e seu datacenter usando o encapsulamento de roteamento genérico (GRE).

- Camada 3 funcionalidade de encaminhamento.

- Border Gateway Protocol (BGP) de roteamento, que permite que você gerencie o roteamento de tráfego de rede entre redes VM seu locatários e seus locais remotos.

Controlador de rede pode colocar diferentes conexões de um locatário em gateways separados. Você pode usar um IP público único para todas as conexões de gateway ou ter diferentes IPs pública para um subconjunto das conexões de. Controlador de rede registra em log todas as configuração de gateway e mudanças de estado, que podem ser usadas para auditoria e solução de problemas.

Para obter mais informações sobre BGP, consulte [Border Gateway Protocol & #40; BGP & #41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md).

Para obter mais informações sobre o Gateway RAS, consulte [RAS Gateway para SDN](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-for-SDN.md).

## <a name="network-controller-deployment-options"></a>Opções de implantação do controlador de rede

Para implantar o controlador de rede usando o System Center Virtual Machine Manager \(VMM\), veja [configurar um controlador de rede SDN na malha VMM](https://technet.microsoft.com/system-center-docs/vmm/scenario/sdn-network-controller).

Para implantar o controlador de rede usando scripts, veja [implantar um Software definidos rede infraestrutura usando Scripts](../../deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md).

Para implantar o controlador de rede usando o Windows PowerShell, veja [implantar o controlador de rede usando o Windows PowerShell](../../deploy/Deploy-Network-Controller-using-Windows-PowerShell.md)
