---
title: Planejar a rede do Hyper-V no Windows Server
description: Descreve o que é necessário para a rede básica no Hyper-V e fornece links para instruções
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: 83c014b917566a78796d061dd88962966ec0c9ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829867"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>Planejar a rede do Hyper-V no Windows Server

>Aplica-se a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019
  
Uma compreensão básica da rede no Hyper-V ajuda você a planejar a rede para máquinas virtuais. Este artigo também aborda algumas considerações de rede ao usar a migração ao vivo e ao usar o Hyper-V com outras funções e recursos de servidor.  
  
## <a name="hyper-v-networking-basics"></a>Noções básicas de rede do Hyper-V  
A rede básica no Hyper-V é bastante simple. Ele usa duas partes: um comutador virtual e um adaptador de rede virtual. Você precisará pelo menos um de cada para estabelecer a rede para uma máquina virtual. O comutador virtual se conecta a qualquer rede baseada em Ethernet. O adaptador de rede virtual se conecta a uma porta no comutador virtual, o que torna possível para uma máquina virtual use uma rede.  
  
A maneira mais fácil para estabelecer um sistema de rede básico é criar um comutador virtual ao instalar o Hyper-V. Em seguida, quando você cria uma máquina virtual, você pode conectá-lo ao comutador. Conectar-se automaticamente ao comutador adiciona um adaptador de rede virtual à máquina virtual. Para obter instruções, consulte [criar um comutador virtual para máquinas virtuais Hyper-V](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  
  
Para lidar com diferentes tipos de rede, você pode adicionar comutadores virtuais e adaptadores de rede virtual. Todos os comutadores fazem parte do host do Hyper-V, mas cada adaptador de rede virtual pertence a apenas uma máquina virtual.  
  
O comutador virtual é um comutador de rede de Ethernet de camada 2 baseado em software. Ele fornece recursos internos para o monitoramento, de controle e segmentar tráfego, bem como segurança e diagnóstico.  Você pode adicionar ao conjunto de recursos internos por meio da instalação do plug-ins, também chamados de *extensões*. Eles estão disponíveis de fornecedores independentes de software. Para obter mais informações sobre o comutador e extensões, consulte [comutador Virtual Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
### <a name="switch-and-network-adapter-choices"></a>Opções de adaptador de rede e comutador  
Hyper-V oferece três tipos de comutadores virtuais e os dois tipos de adaptadores de rede virtual. Você poderá escolher qual de cada desejado quando criá-lo. Você pode usar o Gerenciador do Hyper-V ou o módulo do Hyper-V para o Windows PowerShell para criar e gerenciar os comutadores virtuais e adaptadores de rede virtual. Alguns recursos avançados de rede, os como listas de controle de acesso de porta estendida (ACLs), só podem ser gerenciados usando os cmdlets no módulo Hyper-V.  
  
Você pode fazer algumas alterações a um comutador virtual ou um adaptador de rede virtual depois de criá-lo. Por exemplo, é possível alterar um comutador existente para um tipo diferente, mas fazer isso afeta os recursos de rede de todas as máquinas virtuais conectados a esse comutador.  Portanto, você provavelmente não fazer isso, a menos que você cometeu um erro ou precisa testar algo. Como outro exemplo, você pode se conectar a um adaptador de rede virtual a um comutador diferente, que pode ocorrer se você quiser se conectar a uma rede diferente. Mas, você não pode alterar um adaptador de rede virtual de um tipo para outro. Em vez de alterar o tipo, você poderia adicionar outro adaptador de rede virtual e escolha o tipo apropriado.  
  
Tipos de comutador virtual são:  
  
-   **Comutador virtual externo** -conecta a uma rede com fio, física pela associação a um adaptador de rede física.  
  
-   **Comutador virtual interno** -conecta a uma rede que pode ser usada apenas pelas máquinas virtuais em execução no host que tem o comutador virtual e entre o host e as máquinas virtuais.  
  
-   **Comutador virtual privado** -conecta a uma rede que pode ser usada apenas pelas máquinas virtuais em execução no host que tem o comutador virtual, mas não fornece uma rede entre o host e as máquinas virtuais.  
  
Tipos de adaptadores de rede virtual são:  
  
-   **Adaptador de rede específico do Hyper-V** – disponível para a geração 1 e máquinas virtuais de geração 2. Ele foi desenvolvido especificamente para o Hyper-V e requer um driver que está incluído nos serviços de integração do Hyper-V. Esse tipo de adaptador de rede mais rápida e é a opção recomendada, a menos que você precisa inicializar a rede ou está executando um sistema operacional de convidado sem suporte. O driver necessário é fornecido apenas para sistemas operacionais convidados com suporte. Observe que, no Gerenciador do Hyper-V e os cmdlets de rede, esse tipo é chamado apenas de um adaptador de rede.  
  
-   **Adaptador de rede herdado** : disponível apenas em máquinas virtuais de geração 1. Emula um baseado em Intel 21140 PCI Fast adaptador Ethernet e pode ser usado para inicializar a uma rede, então você pode instalar um sistema operacional de um serviço como serviços de implantação do Windows.  
  
## <a name="hyper-v-networking-and-related-technologies"></a>Sistema de rede do Hyper-V e as tecnologias relacionadas  
Versões recentes do Windows Server introduziu melhorias que oferecem mais opções para configuração de rede do Hyper-V. Por exemplo, Windows Server 2012 introduziu o suporte convergido rede. Isso lhe permite rotear o tráfego de rede por meio de um comutador virtual externo. Windows Server 2016 é construído nessa base permitindo direto memória RDMA (acesso remoto) em adaptadores de rede associados a um comutador virtual do Hyper-V. Você pode usar essa configuração, com ou sem Switch Embedded Teaming (SET). Para obter detalhes, consulte [remotos acesso direto à memória &#40;RDMA&#41; e agrupamento incorporado do comutador &#40;definir&#41;](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
Alguns recursos dependem de configurações de rede específicas ou fazem melhor em determinadas configurações. Considere estas ao planejamento ou atualizar sua infraestrutura de rede.  
  
**Clustering de failover** -é uma prática recomendada para isolar o tráfego de cluster e usar o Hyper-V qualidade de serviço (QoS) no comutador virtual. Para obter detalhes, consulte [recomendações de rede para um Cluster Hyper-V](https://technet.microsoft.com/library/dn550728.aspx)  
  
**Migração ao vivo** -usar opções de desempenho para reduzir a rede e uso da CPU e o tempo necessário para concluir a migração ao vivo. Para obter instruções, consulte [configurar hosts para migração ao vivo sem Clustering de Failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).  
  
**Espaços de armazenamento diretos** -esse recurso se baseia no protocolo de rede SMB3.0 e RDMA. Para obter detalhes, consulte [espaços de armazenamento diretos no Windows Server 2016](../../../storage/storage-spaces/storage-spaces-direct-overview.md).