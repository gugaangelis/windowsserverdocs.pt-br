---
title: Cenários de rede com suporte do Windows Server
description: Este tópico fornece informações sobre novos cenários de rede com suporte no Windows Server 2016 e posterior
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: lizross
author: eross-msft
ms.openlocfilehash: e2e59b70b102e7ca942e2aafb8b216fecd62fc32
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315153"
---
# <a name="windows-server-supported-networking-scenarios"></a>Cenários de rede com suporte do Windows Server

>Aplica-se a: Windows Server \(canal semestral\), Windows Server 2016

Este tópico fornece informações sobre os cenários com e sem suporte que você pode ou não executar com esta versão do Windows Server 2016.  
>[!IMPORTANT]
>Para todos os cenários de produção, use os drivers de hardware mais recentes assinados de seu fabricante de equipamento original \(OEM\) ou fornecedor de hardware independente \(\)de IHV.
  
## <a name="supported-networking-scenarios"></a><a name="bkmk_supp"></a>Cenários de rede com suporte

Esta seção inclui informações sobre os cenários de rede com suporte para o Windows Server 2016 e inclui as categorias de cenário a seguir.  
  
-   [Cenários de rede definida pelo software (SDN)](#bkmk_sdn)  
  
-   [Cenários de plataforma de rede](#bkmk_netp)  
  
-   [Cenários de servidor DNS](#bkmk_dns)  
  
-   [Cenários do IPAM com DHCP e DNS](#bkmk_ipam)  
  
-   [Cenários de agrupamento NIC](#bkmk_nicteam)

- [Mudar a equipe inserida \(definir cenários de\)](#bkmk_set)
  
### <a name="software-defined-networking-sdn-scenarios"></a><a name="bkmk_sdn"></a>Cenários de rede definida pelo software (SDN)
 
Você pode usar a documentação a seguir para implantar cenários de SDN com o Windows Server 2016.  
  
  
-   [Implantar uma infraestrutura de rede definida pelo software usando scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Para obter mais informações, consulte [rede &#40;definida pelo&#41;software Sdn](sdn/software-defined-networking.md).  
  
#### <a name="network-controller-scenarios"></a><a name="bkmk_netc"></a>Cenários do controlador de rede

Os cenários do controlador de rede permitem que você:  
  
-   Implante e gerencie uma instância de vários nós do controlador de rede. Para obter mais informações, consulte [implantar o controlador de rede usando o Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Use o controlador de rede para definir de forma programática a política de rede usando a API REST do northbound.  
  
-   Use o controlador de rede para criar e gerenciar redes virtuais com a virtualização de rede Hyper-V-usando o encapsulamento NVGRE ou VXLAN.  
  
Para obter mais informações, consulte [Controlador de Rede](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="network-function-virtualization-nfv-scenarios"></a><a name="bkmk_netf"></a>Cenários de NFV (virtualização de função de rede)  
Os cenários de NFV permitem que você:  
  
-   Implante e use um balanceador de carga de software para distribuir o tráfego Northbound e Southbound.  
  
-   Implante e use um balanceador de carga de software para distribuir o tráfego Eastbound e Westbound para redes virtuais criadas com virtualização de rede Hyper-V.  
  
-   Implante e use um balanceador de carga de software NAT para redes virtuais criadas com virtualização de rede Hyper-V.  
  
-   Implantar e usar um gateway de encaminhamento de camada 3  
  
-   Implantar e usar um gateway de VPN (rede virtual privada) para túneis de IPsec site a site (IKEv2)  
  
-   Implante e use um gateway de túnel de roteamento genérico (GRE).  
  
-   Implante e configure o roteamento de tráfego dinâmico e de trânsito entre sites usando Border Gateway Protocol (BGP).  
  
-   Configure a redundância M + N para os gateways de camada 3 e site a site e para roteamento BGP.  
  
-   Use o controlador de rede para especificar ACLs em redes virtuais e interfaces de rede.  
  
Para obter mais informações, consulte [virtualização de função de rede](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="network-platform-scenarios"></a><a name="bkmk_netp"></a>Cenários de plataforma de rede

Para os cenários nesta seção, a equipe de rede do Windows Server dá suporte ao uso de qualquer driver certificado do Windows Server 2016. Verifique com sua placa de interface de rede \(o fabricante de\) NIC para garantir que você tenha as atualizações de driver mais recentes.
  
Os cenários de plataforma de rede permitem que você:  
  
-   Use uma NIC convergida para combinar o tráfego RDMA e Ethernet usando um único adaptador de rede.  
  
-   Crie um caminho de dados de baixa latência usando o pacote direto, habilitado no comutador virtual do Hyper-V e um único adaptador de rede.  
  
-   Configure SET para distribuir SMB direto e o tráfego RDMA flui entre até dois adaptadores de rede.  
  
Para obter mais informações, [consulte Remote Direct Memory &#40;Access&#41; RDMA e switch Embedded &#40;Integration&#41;Set](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="hyper-v-virtual-switch-scenarios"></a><a name="bkmk_switch"></a>Cenários de comutador virtual do Hyper-V

Os cenários do comutador virtual do Hyper-V permitem:  
  
-   Criar um comutador virtual do Hyper-V com um vNIC de acesso de memória direta remota (RDMA)  
  
-   Criar um comutador virtual do Hyper-V com o switch Embedded Integration (SET) e o RDMA vNICs  
  
-   Criar uma equipe de conjunto no comutador virtual do Hyper-V  
  
-   Gerenciar uma equipe de conjunto usando comandos do Windows PowerShell  
  
Para obter mais informações, [consulte Remote Direct Memory &#40;Access&#41; RDMA e switch Embedded Integration &#40;Set&#41; ](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="dns-server-scenarios"></a><a name="bkmk_dns"></a>Cenários de servidor DNS

Os cenários de servidor DNS permitem que você:  
  
-   Especificar o gerenciamento de tráfego baseado na localização geográfica usando as políticas de DNS  
  
-   Configurar o DNS de divisão-Brain usando políticas de DNS  
  
-   Aplicar filtros em consultas DNS usando políticas de DNS  
  
-   Configurar o balanceamento de carga de aplicativo usando políticas DNS  
  
-   Especificar respostas de DNS inteligentes com base na hora do dia  
  
-   Configurar políticas de transferência de zona DNS  
  
-   Configurar políticas de servidor DNS em zonas integradas do Active Directory Domain Services (AD DS)  
  
-   Configurar limitação da taxa de resposta  
  
-   Especificar a autenticação baseada em DNS de entidades nomeadas (SUNDANÊS)  
  
-   Configurar o suporte para registros desconhecidos no DNS  
  
Para obter mais informações, consulte os tópicos [o que há de novo no cliente DNS no Windows server 2016](dns/What-s-New-in-DNS-Client.md) e [o que há de novo no servidor DNS no Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="ipam-scenarios-with-dhcp-and-dns"></a><a name="bkmk_ipam"></a>Cenários do IPAM com DHCP e DNS

Os cenários do IPAM permitem que você:  
  
-   Descobrir e administrar servidores DNS e DHCP e endereçamento IP em várias florestas de Active Directory federada  
  
-   Use o IPAM para o gerenciamento centralizado de propriedades de DNS, incluindo zonas e registros de recursos.  
  
-   Defina políticas de controle de acesso baseado em função granulares e delegue usuários ou grupos de usuários do IPAM para gerenciar o conjunto de propriedades DNS que você especificar.  
  
-   Use os comandos do Windows PowerShell para o IPAM para automatizar a configuração de controle de acesso para DHCP e DNS.  
  
    Para obter mais informações, consulte [Manage IPAM](technologies/ipam/Manage-IPAM.md).  
  
### <a name="nic-teaming-scenarios"></a><a name="bkmk_nicteam"></a>Cenários de agrupamento NIC

Os cenários de agrupamento NIC permitem:  
  
-   Criar uma equipe NIC em uma configuração com suporte  
  
-   Excluir uma equipe NIC  
  
-   Adicionar adaptadores de rede à equipe NIC em uma configuração com suporte  
  
-   Remover adaptadores de rede da equipe NIC  
  
> [!NOTE]  
> No Windows Server 2016, você pode usar o agrupamento NIC no Hyper-V, no entanto, em alguns casos, as filas de máquina virtual (VMQ) podem não ser habilitadas automaticamente nos adaptadores de rede subjacentes quando você cria uma equipe NIC. Se isso ocorrer, você poderá usar o seguinte comando do Windows PowerShell para garantir que a VMQ esteja habilitada nos adaptadores membros da equipe NIC: `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Para obter mais informações, consulte [agrupamento NIC](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="switch-embedded-teaming-set-scenarios"></a><a name="bkmk_set"></a>Mudar a equipe inserida \(definir cenários de\)

O conjunto é uma solução de agrupamento NIC alternativa que você pode usar em ambientes que incluem o Hyper-V e a pilha de SDN (rede definida pelo software) no Windows Server 2016. O conjunto integra algumas funcionalidades de agrupamento NIC ao comutador virtual Hyper-V. 

Para obter mais informações, consulte [RDMA (acesso remoto direto à memória) e o comutador inserido de equipe (Set)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="unsupported-networking-scenarios"></a><a name="bkmk_unsupp"></a>Cenários de rede sem suporte  
Os cenários de rede a seguir não têm suporte no Windows Server 2016.  
  
-   Redes virtuais de locatário baseadas em VLAN.  
  
-   Não há suporte para IPv6 na underlay nem na sobreposição.  
  


