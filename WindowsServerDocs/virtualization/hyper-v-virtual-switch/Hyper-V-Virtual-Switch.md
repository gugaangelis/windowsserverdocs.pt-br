---
title: Comutador Virtual Hyper-V
description: Este tópico fornece uma visão geral do comutador Virtual Hyper-V no Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 17962b9bbb90a5ea1b6a4a303c64d72f74be37b5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860917"
---
# <a name="hyper-v-virtual-switch"></a>Comutador Virtual Hyper-V

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico fornece uma visão geral do comutador Virtual Hyper-V, que fornece a capacidade de se conectar a máquinas virtuais \(VMs\) a redes externas para o Hyper\-host V, incluindo a intranet da sua organização e a Internet. 

Você também pode se conectar às redes virtuais no servidor que está executando o Hyper\-V quando você implanta a rede definida pelo Software \(SDN\).

> [!NOTE]  
> Além deste tópico, a seguinte documentação do comutador Virtual Hyper-V está disponível.  
>   
> - [Gerenciar o comutador Virtual Hyper-V](Manage-Hyper-V-Virtual-Switch.md) 
> - [Acesso remoto direto à memória (RDMA) e incorporado do comutador (SET) de agrupamento](RDMA-and-Switch-Embedded-Teaming.md)
> - [Cmdlets de equipe do comutador de rede no Windows PowerShell](https://technet.microsoft.com/library/jj553812.aspx)
> - [O que há de novo no VMM 2016](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [Configurar o malha de rede do VMM](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [Criar redes com o VMM 2012](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-v: Configurar VLANs e a marcação de VLAN](https://social.technet.microsoft.com/wiki/contents/articles/1306.hyper-v-configure-vlans-and-vlan-tagging.aspx)  
> - [Hyper-v: A extensão do comutador virtual WFP deve ser habilitada se for exigida pelas extensões de terceiros](https://social.technet.microsoft.com/wiki/contents/articles/13071.hyper-v-the-wfp-virtual-switch-extension-should-be-enabled-if-it-is-required-by-third-party-extensions.aspx)
>
> Para obter mais informações sobre outras tecnologias de rede, consulte [sistema de rede do Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).
  
Hyper\-comutador Virtual V é um software com base em camada 2 Ethernet comutador de rede que está disponível no Hyper\-V Manager ao instalar o Hyper\-função de servidor V.

Comutador Virtual Hyper-V inclui funcionalidades programaticamente gerenciadas e extensíveis para conectar VMs às redes virtuais e a rede física. Além disso, o Comutador Virtual Hyper-V fornece imposição de política para níveis de segurança, de isolamento e de serviço.  
  
> [!NOTE]  
> O Comutador Virtual Hyper-V dá suporte apenas a Ethernet e não dá suporte a quaisquer outras tecnologias de LAN (rede local) com fio, como Infiniband e Fibre Channel.  
  
Comutador Virtual Hyper-V inclui recursos de isolamento de locatários, modelagem de tráfego, proteção contra máquinas virtuais mal-intencionadas e solução de problemas simplificada. 

Com suporte interno para especificação de Interface de dispositivo de rede \(NDIS\) filtrar os drivers e a Windows Filtering Platform \(WFP\) drivers de texto explicativo, independente de software permite que o comutador Virtual do Hyper-V fornecedores \(ISVs\) criar plug-ins extensíveis, chamados de extensões do comutador Virtual, que pode fornecer recursos de rede e segurança aprimorados. As Extensões de Comutador Virtual que podem ser adicionadas ao Comutador Virtual Hyper-V são listadas no recurso Gerenciador de Comutador Virtual do Gerenciador Hyper-V.
  
Na ilustração a seguir, uma VM tem uma NIC virtual que está conectada para o comutador Hyper-V Virtual por meio de uma porta do comutador.  
  
![Conexões de comutador virtual](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
Recursos do comutador Virtual Hyper-V fornecem mais opções para impor o isolamento de locatários, modelar e controlar tráfego de rede, e aplicar medidas de proteção contra máquinas virtuais mal-intencionadas.

>[!NOTE]
> No Windows Server 2016, uma VM com uma NIC virtual com precisão exibe a taxa de transferência máxima para a NIC virtual. Para exibir a velocidade da NIC virtual em **conexões de rede**, clique com botão direito no ícone NIC virtual desejado e, em seguida, clique em **Status**. A NIC virtual **Status** caixa de diálogo é aberta. Na **Conexão**, o valor de **velocidade** coincide com a velocidade do NIC físico instalado no servidor.
  
## <a name="bkmk_apps"></a>Usos para o comutador Virtual Hyper-V

A seguir estão alguns cenários de caso de uso para o comutador Virtual Hyper-V.

**Exibindo estatísticas**: um desenvolvedor em um fornecedor de nuvem hospedado implementa um pacote de gerenciamento que exibe o estado atual do comutador virtual Hyper-V. O pacote de gerenciamento pode consultar as funcionalidades atuais em todo o comutador, as definições de configuração e estatísticas de redes de portas individuais usando a WMI. Em seguida, o status do comutador é apresentado para dar aos administradores uma visualização rápida do estado do comutador.  
  
**Rastreamento de recursos**: uma empresa de hospedagem está vendendo serviços com preços estipulados de acordo com o nível de associação. Vários níveis de associação incluem diferentes níveis de desempenho de rede. O administrador aloca recursos para atender aos contratos de nível de serviço de maneira que equilibre a disponibilidade da rede. O administrador controla programaticamente informações como o uso atual da largura de banda atribuída e o número de VM (máquina virtual) atribuído a fila de máquina virtual (VMQ) ou canais IOV. O mesmo programa também registra periodicamente os recursos em uso, além dos recursos atribuídos por VM para rastreamento ou recursos de dupla entrada.  
  
**Gerenciando a ordem das extensões do comutador**: uma empresa instalou extensões no host Hyper-V para monitorar o tráfego e relatar detecção de intrusões. Durante a manutenção, algumas extensões podem ser atualizadas causando a alteração da ordem das extensões. Um programa de script simples é executado para reordenar as extensões após as atualizações.  
  
**Extensão de encaminhamento gerencia a ID de VLAN**: uma importante empresa de comutadores está construindo uma extensão de encaminhamento que se aplica a todas as políticas de rede. Um elemento que é gerenciado são as IDs de VLAN (rede local virtual). O comutador virtual cede o controle da VLAN para uma extensão de encaminhamento. Instalação da empresa de comutadores chama programaticamente uma Windows Management Instrumentation (WMI) aplicativo programação interface (API) que habilita a transparência, informando que o comutador Hyper-V Virtual passar e nenhuma ação nas marcas de VLAN.  
  
## <a name="bkmk_func"></a>Funcionalidade do comutador Virtual do Hyper-V
 
Estes são alguns dos principais recursos incluídos no Comutador Virtual Hyper-V:  
  
-   **Proteção contra envenenamento de ARP/ND (falsificação)**: fornece proteção contra uma VM mal-intencionada que usa falsificação de protocolo ARP para roubar endereços IP de outras VMs. Fornece proteção contra ataques que podem ser iniciados para IPv6 usando falsificação ND (Descoberta de Vizinhos).  
  
-   **Proteção DHCP**: protege contra uma VM mal-intencionada que representa a si mesma como servidor DHCP para ataques “man-in-the-middle”.  
  
-   **ACLs de porta**: fornecem filtragem baseada em endereços/intervalos MAC (Controle de Acesso à Mídia) ou IP, o que permite configurar o isolamento de rede virtual.  
  
-   **Modo de tronco para uma VM**: permite aos administradores configurar uma VM específica como um dispositivo virtual e direcionar o tráfego de várias VLANs para essa VM.  
  
-   **Monitoramento de tráfego de rede**: permite aos administradores examinar o tráfego que está atravessando o comutador de rede.  
  
-   **VLAN isolada (privada)**: permite aos administradores segregar o tráfego de várias VLANs para estabelecer mais facilmente comunidades de locatários isoladas.  
  
A seguir está uma lista de funcionalidades que melhoram a capacidade de uso do Comutador Virtual Hyper-V:  
  
-   **Limite de largura de banda e suporte de intermitente**: o mínimo de largura de banda garante a quantidade de largura de banda reservada. O máximo de largura de banda limita a quantidade de largura de banda que uma VM pode consumir.  
  
-   **Explicit Congestion marcação ECN (notificação) suporte**:  Marcação, também conhecida como Dctcp (Data Center TCP), ECN permite que o comutador físico e sistema operacional regulem o fluxo de tráfego de tal forma que os recursos de buffer do comutador não sejam inundados, o que resulta na taxa de transferência de aumento de tráfego.  
  
-   **Diagnóstico**: o diagnóstico permite o fácil rastreamento e monitoramento de eventos e pacotes através do comutador virtual.
