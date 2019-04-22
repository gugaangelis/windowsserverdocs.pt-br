---
title: Cenários de rede com suporte do Windows Server
description: Este tópico fornece informações sobre novos cenários de rede com suporte no Windows Server 2016 e posterior
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.date: ''
ms.assetid: 6de4232d-b0b3-4e43-8735-ebae35ae4f9f
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 85f73f1f7caf833d23d3d693c0d754f52c4aa27d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59812227"
---
# <a name="windows-server-supported-networking-scenarios"></a>Cenários de rede com suporte do Windows Server

>Aplica-se a: Windows Server \(canal semestral\), Windows Server 2016

Este tópico fornece informações sobre cenários compatíveis e sem suportados que você pode ou não é possível executar com esta versão do Windows Server 2016.  
>[!IMPORTANT]
>Para todos os cenários de produção, use os drivers mais recentes de hardware assinado do fabricante do equipamento original \(OEM\) ou fornecedor independente de hardware \(IHV\).
  
## <a name="bkmk_supp"></a>Suporte para cenários de rede

Esta seção inclui informações sobre os cenários de rede com suporte para o Windows Server 2016 e inclui as seguintes categorias de cenário.  
  
-   [Cenários do software Defined de Networking (SDN)](#bkmk_sdn)  
  
-   [Cenários de plataforma de rede](#bkmk_netp)  
  
-   [Cenários de servidor DNS](#bkmk_dns)  
  
-   [Cenários do IPAM com o DHCP e DNS](#bkmk_ipam)  
  
-   [Cenários de agrupamento NIC](#bkmk_nicteam)

- [Agrupamento incorporado do comutador \(definir\) cenários](#bkmk_set)
  
### <a name="bkmk_sdn"></a>Cenários do software Defined de Networking (SDN)
 
Você pode usar a documentação a seguir para implantar cenários SDN com o Windows Server 2016.  
  
  
-   [Implantar uma infraestrutura de rede definida por Software usando scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Para obter mais informações, consulte [rede definida pelo Software &#40;SDN&#41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>Cenários de controlador de rede

Os cenários de controlador de rede permitem que você:  
  
-   Implantar e gerenciar uma instância de vários nós de controlador de rede. Para obter mais informações, consulte [implantar o controlador de rede usando o Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Use o controlador de rede para definir programaticamente a diretiva de rede usando a API Northbound do REST.  
  
-   Use o controlador de rede para criar e gerenciar redes virtuais com virtualização de rede do Hyper-V – encapsulamento NVGRE ou VXLAN.  
  
Para obter mais informações, consulte [Controlador de Rede](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="bkmk_netf"></a>Cenários de função NFV (virtualização) da rede  
Os cenários NFV permitem que você:  
  
-   Implantar e usar um balanceador de carga de software para distribuir o tráfego northbound e southbound.  
  
-   Implantar e usar um balanceador de carga de software para distribuir o tráfego eastbound e westbound para redes virtuais criadas com a virtualização de rede do Hyper-V.  
  
-   Implantar e usar um balanceador de carga de software NAT para redes virtuais criadas com a virtualização de rede do Hyper-V.  
  
-   Implantar e usar um gateway de encaminhamento de camada 3  
  
-   Implantar e usar um gateway de rede virtual privada (VPN) para os túneis de IPsec (IKEv2) site a site  
  
-   Implantar e usar um gateway de encapsulamento de roteamento genérico (GRE).  
  
-   Implantar e configurar o roteamento dinâmico e roteamento de tráfego entre sites usando o Border Gateway Protocol (BGP).  
  
-   Configure redundância M + N para gateways site a site e de camada 3 e para roteamento de BGP.  
  
-   Use o controlador de rede para especificar as ACLs em redes virtuais e adaptadores de rede.  
  
Para obter mais informações, consulte [a virtualização de rede função](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_netp"></a>Cenários de plataforma de rede

Para os cenários nesta seção, a rede do Windows Server equipe suporta o uso de qualquer driver certificados do Windows Server 2016. Entre em contato com sua placa de interface de rede \(NIC\) fabricante para garantir que você tenha as atualizações mais recentes do driver.
  
Os cenários de plataforma de rede permitem que você:  
  
-   Use uma NIC convergida para combinar o tráfego RDMA e Ethernet, usando um único adaptador de rede.  
  
-   Crie um caminho de dados de baixa latência usando o pacote direto, ativada no comutador Virtual do Hyper-V e um único adaptador de rede.  
  
-   Configure conjunto para distribuir fluxos de tráfego SMB Direct e o RDMA entre até dois adaptadores de rede.  
  
Para obter mais informações, consulte [remotos acesso direto à memória &#40;RDMA&#41; e agrupamento incorporado do comutador &#40;definir&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Cenários de comutador Virtual Hyper-V

Os cenários de comutador Virtual Hyper-V permite que você:  
  
-   Criar um comutador Virtual do Hyper-V com uma vNIC direto memória RDMA (acesso remoto)  
  
-   Criar um comutador Virtual do Hyper-V com vNICs RDMA e Switch Embedded Teaming (SET)  
  
-   Criar um conjunto de agrupamento no comutador Virtual Hyper-V  
  
-   Gerenciar uma equipe de conjunto, usando comandos do Windows PowerShell  
  
Para obter mais informações, consulte [remotos acesso direto à memória &#40;RDMA&#41; e agrupamento incorporado do comutador &#40;definir&#41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>Cenários de servidor DNS

Cenários de servidor DNS permitem que você:  
  
-   Especifique a localização geográfica com base em gerenciamento de tráfego usando políticas de DNS  
  
-   Configurar o DNS com partição de rede usando políticas de DNS  
  
-   Aplicar filtros em consultas DNS usando as políticas de DNS  
  
-   Configurar balanceamento de carga do aplicativo usando políticas de DNS  
  
-   Especificar que respostas de DNS inteligente com base na hora do dia  
  
-   Configurar políticas de transferência de zona DNS  
  
-   Configurar zonas do DNS server políticas em serviços de domínio Active Directory (AD DS) integradas  
  
-   Configurar a taxa de resposta de limitação  
  
-   Especificar a autenticação com base em DNS de entidades nomeadas (painel)  
  
-   Configurar o suporte para registros desconhecidos no DNS  
  
Para obter mais informações, consulte os tópicos [o que há de novo no cliente DNS no Windows Server 2016](dns/What-s-New-in-DNS-Client.md) e [o que há de novo no servidor DNS no Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="bkmk_ipam"></a>Cenários do IPAM com o DHCP e DNS

Os cenários do IPAM permite que você:  
  
-   Descobrir e administrar servidores DNS e DHCP e endereçamento IP através de várias florestas do Active Directory federadas  
  
-   Use o IPAM para gerenciamento centralizado de propriedades DNS, incluindo zonas e registros de recursos.  
  
-   Definir políticas de controle de acesso baseado em função granular e delegar usuários do IPAM ou grupos de usuários para gerenciar o conjunto de propriedades DNS que você especificar.  
  
-   Use os comandos do Windows PowerShell para IPAM para automatizar a configuração de controle de acesso para o DHCP e DNS.  
  
    Para obter mais informações, consulte [Manage IPAM](technologies/ipam/Manage-IPAM.md).  
  
### <a name="bkmk_nicteam"></a>Cenários de agrupamento NIC

Os cenários de agrupamento NIC permite que você:  
  
-   Criar um agrupamento NIC em uma configuração com suporte  
  
-   Excluir um agrupamento NIC  
  
-   Adicionar adaptadores de rede para o agrupamento NIC em uma configuração com suporte  
  
-   Remover adaptadores de rede da equipe NIC  
  
> [!NOTE]  
> No Windows Server 2016, você pode usar o agrupamento NIC no Hyper-V, no entanto, em alguns casos as filas de máquina Virtual (VMQ) talvez não habilita automaticamente nos adaptadores de rede subjacente quando você cria uma equipe NIC. Se isso ocorrer, você pode usar o seguinte comando do Windows PowerShell para garantir que a VMQ está habilitado nos adaptadores de membro da equipe de NIC: `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Para obter mais informações, consulte [agrupamento NIC](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="bkmk_set"></a>Agrupamento incorporado do comutador \(definir\) cenários

CONJUNTO é uma solução alternativa do agrupamento NIC que você pode usar em ambientes que incluem o Hyper-V e a pilha de Software Defined Networking (SDN) no Windows Server 2016. CONJUNTO integra algumas funcionalidades de agrupamento NIC no comutador Virtual do Hyper-V. 

Para obter mais informações, consulte [acesso direto à memória remoto (RDMA) e Switch Embedded Teaming (SET)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>Não há suporte para cenários de rede  
Não há suporte para os seguintes cenários de rede no Windows Server 2016.  
  
-   Redes virtuais de locatário com base em VLAN.  
  
-   Não há suporte para IPv6 na base ou sobreposição.  
  


