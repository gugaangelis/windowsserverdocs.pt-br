---
title: Comutador Virtual Hyper-V
description: Este tópico fornece uma visão geral do comutador virtual do Hyper-V no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: article
ms.assetid: 398440ac-5988-41ce-b91e-eab343a255d3
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c508005af67e9dd5b0c9a22693aca25eb19e8e48
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366835"
---
# <a name="hyper-v-virtual-switch"></a>Comutador Virtual Hyper-V

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico fornece uma visão geral do comutador virtual do Hyper-V, que fornece a capacidade de conectar máquinas virtuais \(VMs @ no__t-1 a redes que são externas ao host Hyper @ no__t-2V, incluindo a intranet da sua organização e a Internet. 

Você também pode se conectar a redes virtuais no servidor que executa o Hyper @ no__t-0V ao implantar a rede definida pelo software \(SDN @ no__t-2.

> [!NOTE]  
> Além deste tópico, a documentação do comutador virtual do Hyper-V a seguir está disponível.  
>   
> - [Gerenciar o comutador virtual do Hyper-V](Manage-Hyper-V-Virtual-Switch.md) 
> - [Acesso Remoto Direto à Memória (RDMA) e Agrupamento incorporado de comutador (SET)](RDMA-and-Switch-Embedded-Teaming.md)
> - [Cmdlets da equipe do comutador de rede no Windows PowerShell](https://technet.microsoft.com/library/jj553812.aspx)
> - [O que há de novo no VMM 2016](https://docs.microsoft.com/system-center/vmm/whats-new#networking)
> - [Configurar a malha de rede do VMM](https://docs.microsoft.com/system-center/vmm/manage-networks)
> - [Criar redes com o VMM 2012](https://social.technet.microsoft.com/wiki/contents/articles/3140.create-networks-with-vmm-2012.aspx)  
> - [Hyper-V: Configurar VLANs e marcação de VLAN @ no__t-0  
> - [Hyper-V: A extensão do comutador virtual WFP deve ser habilitada se for exigida por extensões de terceiros @ no__t-0
>
> Para obter mais informações sobre outras tecnologias de rede, consulte [Networking in Windows Server 2016](https://docs.microsoft.com/windows-server/networking/networking).
  
O comutador virtual Hyper @ no__t-0V é um comutador de rede Ethernet de camada 2 baseado em software que está disponível no Hyper @ no__t-1V Manager quando você instala a função de servidor Hyper @ no__t-2V.

O comutador virtual do Hyper-V inclui recursos gerenciados e extensíveis programaticamente para conectar VMs a redes virtuais e à rede física. Além disso, o Comutador Virtual Hyper-V fornece imposição de política para níveis de segurança, de isolamento e de serviço.  
  
> [!NOTE]  
> O Comutador Virtual Hyper-V dá suporte apenas a Ethernet e não dá suporte a quaisquer outras tecnologias de LAN (rede local) com fio, como Infiniband e Fibre Channel.  
  
O comutador virtual do Hyper-V inclui recursos de isolamento de locatários, formatação de tráfego, proteção contra máquinas virtuais mal-intencionadas e solução de problemas simplificada. 

Com suporte interno para especificação de interface de dispositivo de rede \(NDIS @ no__t-1 drivers de filtro e plataforma de filtragem do Windows \(WFP @ no__t-3 drivers de texto explicativo, o comutador virtual do Hyper-V habilita fornecedores de software independentes \(ISVs @ no__ t-5 para criar plug-ins extensíveis, chamados de extensões de comutador virtual, que podem fornecer recursos de segurança e rede aprimorados. As Extensões de Comutador Virtual que podem ser adicionadas ao Comutador Virtual Hyper-V são listadas no recurso Gerenciador de Comutador Virtual do Gerenciador Hyper-V.
  
Na ilustração a seguir, uma VM tem uma NIC virtual que está conectada ao comutador virtual do Hyper-V por meio de uma porta de comutador.  
  
![Conexões do comutador virtual](../media/Hyper-V-Virtual-Switch/Vswitch_01.jpg)  
  
Os recursos do comutador virtual do Hyper-V fornecem mais opções para impor o isolamento do locatário, Formatar e controlar o tráfego de rede e empregar medidas de proteção contra VMs mal-intencionadas.

>[!NOTE]
> No Windows Server 2016, uma VM com uma NIC virtual exibe com precisão a taxa de transferência máxima para a NIC virtual. Para exibir a velocidade da NIC virtual em **conexões de rede**, clique com o botão direito do mouse no ícone NIC virtual desejado e clique em **status**. A caixa de diálogo **status** da NIC virtual é aberta. Em **conexão**, o valor de **velocidade** corresponde à velocidade da NIC física instalada no servidor.
  
## <a name="bkmk_apps"></a>Usos para o comutador virtual do Hyper-V

A seguir estão alguns cenários de caso de uso para o comutador virtual do Hyper-V.

**Exibindo estatísticas**: um desenvolvedor em um fornecedor de nuvem hospedado implementa um pacote de gerenciamento que exibe o estado atual do comutador virtual Hyper-V. O pacote de gerenciamento pode consultar as funcionalidades atuais em todo o comutador, as definições de configuração e estatísticas de redes de portas individuais usando a WMI. Em seguida, o status do comutador é apresentado para dar aos administradores uma visualização rápida do estado do comutador.  
  
**Acompanhamento de recursos**: uma empresa de hospedagem está vendendo serviços com preços estipulados de acordo com o nível de associação. Vários níveis de associação incluem diferentes níveis de desempenho de rede. O administrador aloca recursos para atender aos contratos de nível de serviço de maneira que equilibre a disponibilidade da rede. O administrador rastreia de forma programática informações como o uso atual da largura de banda atribuída e o número de canais da fila de máquinas virtuais (VMQ) ou de uma VM (máquina virtual) atribuídas. O mesmo programa também registra periodicamente os recursos em uso, além dos recursos atribuídos por VM para rastreamento ou recursos de dupla entrada.  
  
**Gerenciando a ordem das extensões de comutador**: uma empresa instalou extensões no host Hyper-V para monitorar o tráfego e relatar detecção de intrusões. Durante a manutenção, algumas extensões podem ser atualizadas causando a alteração da ordem das extensões. Um programa de script simples é executado para reordenar as extensões após as atualizações.  
  
A **extensão de encaminhamento gerencia a ID de VLAN**: uma importante empresa de comutadores está construindo uma extensão de encaminhamento que se aplica a todas as políticas de rede. Um elemento que é gerenciado são as IDs de VLAN (rede local virtual). O comutador virtual cede o controle da VLAN para uma extensão de encaminhamento. A instalação do switch Company chama programaticamente uma API (interface de programação de aplicativo) Instrumentação de Gerenciamento do Windows (WMI) que ativa a transparência, informando ao comutador virtual Hyper-V que deve passar e não executar nenhuma ação nas marcas de VLAN.  
  
## <a name="bkmk_func"></a>Funcionalidade do comutador virtual do Hyper-V
 
Estes são alguns dos principais recursos incluídos no Comutador Virtual Hyper-V:  
  
-   **Proteção de envenenamento ARP/nd (falsificação)** : fornece proteção contra uma VM mal-intencionada que usa falsificação de protocolo ARP para roubar endereços IP de outras VMs. Fornece proteção contra ataques que podem ser iniciados para IPv6 usando falsificação ND (Descoberta de Vizinhos).  
  
-   **Proteção**do proteção DHCP: protege contra uma VM mal-intencionada que representa a si mesma como servidor DHCP para ataques “man-in-the-middle”.  
  
-   **ACLs de porta**: fornecem filtragem baseada em endereços/intervalos MAC (Controle de Acesso à Mídia) ou IP, o que permite configurar o isolamento de rede virtual.  
  
-   **Modo de tronco para uma VM**: permite aos administradores configurar uma VM específica como um dispositivo virtual e direcionar o tráfego de várias VLANs para essa VM.  
  
-   **Monitoramento de tráfego de rede**: permite aos administradores examinar o tráfego que está atravessando o comutador de rede.  
  
-   **VLAN isolada (privada)** : permite aos administradores segregar o tráfego de várias VLANs para estabelecer mais facilmente comunidades de locatários isoladas.  
  
A seguir está uma lista de funcionalidades que melhoram a capacidade de uso do Comutador Virtual Hyper-V:  
  
-   **Limite de largura de banda e suporte a intermitência**: o mínimo de largura de banda garante a quantidade de largura de banda reservada. O máximo de largura de banda limita a quantidade de largura de banda que uma VM pode consumir.  
  
-   **Suporte à marcação de ECN (notificação de congestionamento explícito)** :  A marcação de ECN, também conhecida como CenterTCP de dados (DCTCP), permite que o comutador físico e o sistema operacional regulam o fluxo de tráfego de modo que os recursos de buffer do comutador não sejam inundados, o que resulta em maior taxa de transferência de tráfego.  
  
-   **Diagnóstico**: o diagnóstico permite o fácil rastreamento e monitoramento de eventos e pacotes através do comutador virtual.
