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
ms.openlocfilehash: 70198f97c4ec39de4b78de28ab196dc3e86a684c
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="windows-server-supported-networking-scenarios"></a>Cenários de rede com suporte do Windows Server

>Aplica-se a: Windows Server \(Semi-Annual Channel\), Windows Server 2016

Este tópico fornece informações sobre cenários com suporte e não suportados que você pode ou não é possível executar com esta versão do Windows Server 2016.  
>[!IMPORTANT]
>Para todos os cenários de produção, use os drivers mais recentes assinado hardware do seu fabricante de equipamento original fornecedor de hardware independente ou \(OEM\) \(IHV\).
  
## <a name="bkmk_supp"></a>Suporte a cenários de rede

Esta seção inclui informações sobre os cenários de redes com suporte para Windows Server 2016 e inclui as seguintes categorias de cenário.  
  
-   [Cenários de definido de rede (SDN) de software](#bkmk_sdn)  
  
-   [Cenários de plataforma de rede](#bkmk_netp)  
  
-   [Cenários de servidor DNS](#bkmk_dns)  
  
-   [Cenários IPAM com DHCP e DNS](#bkmk_ipam)  
  
-   [Cenários de agrupamento de NIC](#bkmk_nicteam)

- [Cenários de agrupamento Embedded \(SET\) switch](#bkmk_set)
  
### <a name="bkmk_sdn"></a>Cenários de definido de rede (SDN) de software
 
Você pode usar a documentação a seguir para implantar cenários SDN com o Windows Server 2016.  
  
  
-   [Implante uma infraestrutura de rede definidos do Software usando scripts](sdn/deploy/Deploy-a-Software-Defined-Network-infrastructure-using-scripts.md)  
  
Para obter mais informações, consulte [Software de rede definidos & #40; SDN & #41;](sdn/software-defined-networking.md).  
  
#### <a name="bkmk_netc"></a>Cenários de controlador de rede

Os cenários de controlador de rede permitem que você:  
  
-   Implantar e gerenciar uma instância de vários nós do controlador de rede. Para obter mais informações, consulte [implantar o controlador de rede usando o Windows PowerShell](sdn/deploy/Deploy-Network-Controller-using-Windows-PowerShell.md).  
  
-   Use o controlador de rede para definir a política de rede de forma programática usando a API de REST em sentido norte.  
  
-   Use o controlador de rede para criar e gerenciar redes virtuais com a virtualização de rede do Hyper-V - usando o encapsulamento NVGRE ou VXLAN.  
  
Para obter mais informações, consulte [rede controlador](sdn/technologies/network-controller/Network-Controller.md).  
  
#### <a name="bkmk_netf"></a>Cenários de virtualização de função (NFV) de rede  
Os cenários NFV Network permitem que você:  
  
-   Implantar e usar um balanceador de carga de software para distribuir o tráfego em sentido Norte e southbound.  
  
-   Implantar e usar um balanceador de carga de software para distribuir o tráfego eastbound e westbound para redes virtuais criadas com a virtualização de rede do Hyper-V.  
  
-   Implantar e usar um balanceador de carga de software NAT para redes virtuais criadas com a virtualização de rede do Hyper-V.  
  
-   Implantar e usar um gateway de encaminhamento de camada 3  
  
-   Implantar e usar um gateway de rede virtual privada (VPN) para túneis de IPsec (IKEv2)-to-site  
  
-   Implantar e usar um gateway de encapsulamento de roteamento genérico (GRE).  
  
-   Implante e configure o roteamento dinâmico e trânsito roteamento entre sites que usam o protocolo de Gateway de borda (BGP).  
  
-   Configure a redundância M + N para Layer 3 e gateways to-site e para roteamento de BGP.  
  
-   Use o controlador de rede para especificar ACLs nas redes virtuais e interfaces de rede.  
  
Para obter mais informações, consulte [rede função virtualização](sdn/technologies/network-function-virtualization/Network-Function-Virtualization.md).  
  
### <a name="bkmk_netp"></a>Cenários de plataforma de rede

Para os cenários nesta seção de rede do Windows Server equipe permite o uso de qualquer driver certificado do Windows Server 2016. Entre em contato com o fabricante da placa \(NIC\) rede interface para garantir que você tenha as atualizações mais recentes do driver.
  
Os cenários de plataforma de rede permitem que você:  
  
-   Use uma NIC convergida para combinar tráfego RDMA e Ethernet usando um único adaptador de rede.  
  
-   Crie um caminho de dados de baixa latência, usando o pacote Direta, habilitada no Switch Virtual Hyper-V e um adaptador de rede.  
  
-   Configure conjunto a disseminação de fluxos de tráfego SMB direto e RDMA entre até dois adaptadores de rede.  
  
Para obter mais informações, consulte [remoto acesso direto à memória & #40; RDMA & #41; e alternar agrupamento Embedded & #40; Definir & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md).  
  
#### <a name="bkmk_switch"></a>Cenários de comutador Virtual Hyper-V

Os cenários do Hyper-V Virtual Switch permitem que você:  
  
-   Criar um comutador Virtual Hyper-V com um vNIC remota direta memória acesso RDMA)  
  
-   Criar um comutador Virtual Hyper-V com agrupamento Embedded Switch (conjunto) e RDMA vNICs  
  
-   Criar uma conjunto de equipe em Hyper-V Virtual Switch  
  
-   Gerenciar um time de conjunto usando comandos do Windows PowerShell  
  
Para obter mais informações, consulte [remoto acesso direto à memória & #40; RDMA & #41; e alternar agrupamento Embedded & #40; Definir & #41;](../virtualization/hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
### <a name="bkmk_dns"></a>Cenários de servidor DNS

Cenários de servidor DNS permitem que você:  
  
-   Especificar a localização geográfica com base em gerenciamento de tráfego usando políticas de DNS  
  
-   Configurar uma DNS usando políticas de DNS  
  
-   Aplicar filtros nas consultas do DNS usando políticas de DNS  
  
-   Configurar balanceamento de carga do aplicativo usando políticas de DNS  
  
-   Especifique inteligente DNS respostas com base na hora do dia  
  
-   Configurar as políticas de transferência de zona de DNS  
  
-   Configurar zonas de políticas nos serviços de domínio do Active Directory (AD DS) integradas do servidor DNS  
  
-   Configurar resposta taxa limitando  
  
-   Especificar a autenticação baseada em DNS de entidades nomeadas (DANE)  
  
-   Configurar o suporte para registros desconhecido no DNS  
  
Para obter mais informações, consulte os tópicos [What's New no cliente DNS no Windows Server 2016](dns/What-s-New-in-DNS-Client.md) e [What's New no servidor DNS no Windows Server 2016](dns/What-s-New-in-DNS-Server.md).  
  
### <a name="bkmk_ipam"></a>Cenários IPAM com DHCP e DNS

Os cenários IPAM permitem que você:  
  
-   Descobrir e administrar servidores DNS e DHCP e endereçamento IP em várias florestas do Active Directory federadas  
  
-   Use IPAM para o gerenciamento centralizado de propriedades DNS, incluindo zonas e registros de recurso.  
  
-   Definir políticas de controle de acesso baseado em função granular e delegar IPAM ou grupos de usuários para gerenciar o conjunto de propriedades DNS que você especificar.  
  
-   Use os comandos do Windows PowerShell para IPAM para automatizar a configuração de controle de acesso para DHCP e DNS.  
  
    Para obter mais informações, consulte [gerenciar IPAM](technologies/ipam/Manage-IPAM.md).  
  
### <a name="bkmk_nicteam"></a>Cenários de agrupamento de NIC

Os cenários de agrupamento de NIC permitem que você:  
  
-   Criar uma equipe de placa de rede em uma configuração com suporte  
  
-   Excluir uma equipe NIC  
  
-   Adicione os adaptadores de rede para a equipe de placa de rede em uma configuração com suporte  
  
-   Remover adaptadores de rede da equipe do NIC  
  
> [!NOTE]  
> No Windows Server 2016, você pode usar o agrupamento de NIC no Hyper-V, mas em alguns casos filas de máquina Virtual (VMQ) talvez não habilitam automaticamente nos adaptadores de rede subjacente quando você cria uma equipe de NIC. Se isso ocorrer, você pode usar o seguinte comando do Windows PowerShell para garantir que VMQ está habilitado nos adaptadores de membro da equipe de NIC: `Set-NetAdapterVmq -Name <NetworkAdapterName> -Enable`  

Para obter mais informações, consulte [NIC agrupamento](technologies/nic-teaming/NIC-Teaming.md). 

### <a name="bkmk_set"></a>Cenários de agrupamento Embedded \(SET\) switch

CONJUNTO é uma solução alternativa NIC agrupamento que você pode usar em ambientes que incluem o Hyper-V e a pilha de rede definidos Software (SDN) no Windows Server 2016. CONJUNTO integra algumas funcionalidades de agrupamento de NIC o Hyper-V Virtual Switch. 

Para obter mais informações, consulte [acesso direto de memória remoto (RDMA) e agrupamento Embedded Switch (conjunto)](https://technet.microsoft.com/windows-server-docs/networking/technologies/hyper-v-virtual-switch/rdma-and-switch-embedded-teaming)
  
 
  
## <a name="bkmk_unsupp"></a>Não há suporte para cenários de rede  
Não há suporte para os seguintes cenários de redes no Windows Server 2016.  
  
-   Redes virtuais com base em VLAN locatário.  
  
-   Não há suporte para o IPv6 na base ou sobreposição.  
  


