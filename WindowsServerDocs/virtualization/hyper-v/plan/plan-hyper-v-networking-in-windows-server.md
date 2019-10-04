---
title: Planejar a rede do Hyper-V no Windows Server
description: Descreve o que é necessário para a rede básica no Hyper-V e fornece links para instruções
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
author: KBDAzure
ms.author: kathydav
ms.date: 10/04/2016
ms.openlocfilehash: f09bc82d0dd47d3393dd05dcf03913db11e4c335
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71392509"
---
# <a name="plan-for-hyper-v-networking-in-windows-server"></a>Planejar a rede do Hyper-V no Windows Server

>Aplica-se a: Microsoft Hyper-V Server 2016, Windows Server 2016, Microsoft Hyper-V Server 2019, Windows Server 2019
  
Uma compreensão básica da rede no Hyper-V ajuda você a planejar a rede para máquinas virtuais. Este artigo também aborda algumas considerações de rede ao usar a migração dinâmica e ao usar o Hyper-V com outros recursos e funções de servidor.  
  
## <a name="hyper-v-networking-basics"></a>Noções básicas de rede do Hyper-V  
A rede básica no Hyper-V é bem simples. Ele usa duas partes: um comutador virtual e um adaptador de rede virtual. Você precisará de pelo menos um de cada para estabelecer a rede para uma máquina virtual. O comutador virtual conecta-se a qualquer rede baseada em Ethernet. O adaptador de rede virtual conecta-se a uma porta no comutador virtual, o que torna possível que uma máquina virtual use uma rede.  
  
A maneira mais fácil de estabelecer a rede básica é criar um comutador virtual ao instalar o Hyper-V. Em seguida, ao criar uma máquina virtual, você pode conectá-la ao comutador. Conectar-se ao comutador adiciona automaticamente um adaptador de rede virtual à máquina virtual. Para obter instruções, consulte [criar um comutador virtual para máquinas virtuais do Hyper-V](../get-started/Create-a-virtual-switch-for-Hyper-V-virtual-machines.md).  
  
Para lidar com diferentes tipos de rede, você pode adicionar comutadores virtuais e adaptadores de rede virtual. Todas as opções fazem parte do host Hyper-V, mas cada adaptador de rede virtual pertence a apenas uma máquina virtual.  
  
O comutador virtual é um comutador de rede Ethernet de camada 2 baseado em software. Ele fornece recursos internos para monitorar, controlar e segmentar o tráfego, bem como segurança e diagnóstico.  Você pode adicionar ao conjunto de recursos internos instalando plug-ins, também chamados de *extensões*. Eles estão disponíveis de fornecedores de software independentes. Para obter mais informações sobre o comutador e as extensões, consulte [comutador virtual do Hyper-V](../../hyper-v-virtual-switch/Hyper-V-Virtual-Switch.md).  
  
### <a name="switch-and-network-adapter-choices"></a>Opções de adaptador de rede e de comutador  
O Hyper-V oferece três tipos de comutadores virtuais e dois tipos de adaptadores de rede virtual. Você escolherá qual delas deseja ao criá-la. Você pode usar o Gerenciador do Hyper-V ou o módulo do Hyper-V para Windows PowerShell para criar e gerenciar comutadores virtuais e adaptadores de rede virtual. Alguns recursos avançados de rede, como ACLs (listas de controle de acesso) de porta estendidas, só podem ser gerenciados usando cmdlets no módulo do Hyper-V.  
  
Você pode fazer algumas alterações em um comutador virtual ou adaptador de rede virtual depois de criá-lo. Por exemplo, é possível alterar um comutador existente para um tipo diferente, mas isso afeta os recursos de rede de todas as máquinas virtuais conectadas a esse comutador.  Portanto, você provavelmente não fará isso, a menos que tenha cometido um erro ou precise testar algo. Como outro exemplo, você pode conectar um adaptador de rede virtual a um comutador diferente, que você pode fazer se quiser se conectar a uma rede diferente. Mas, você não pode alterar um adaptador de rede virtual de um tipo para outro. Em vez de alterar o tipo, você adicionaria outro adaptador de rede virtual e escolheria o tipo apropriado.  
  
Os tipos de comutador virtual são:  
  
-   **Comutador virtual externo** – conecta-se a uma rede física com fio ligando-se a um adaptador de rede física.  
  
-   **Comutador virtual interno** – conecta-se a uma rede que pode ser usada somente pelas máquinas virtuais em execução no host que tem o comutador virtual e entre o host e as máquinas virtuais.  
  
-   **Comutador virtual privado** – conecta-se a uma rede que pode ser usada somente pelas máquinas virtuais em execução no host que tem o comutador virtual, mas não fornece rede entre o host e as máquinas virtuais.  
  
Os tipos de adaptadores de rede virtual são:  
  
-   **Adaptador de rede específico do Hyper-V** – disponível para máquinas virtuais de geração 1 e de geração 2. Ele foi projetado especificamente para o Hyper-V e requer um driver incluído nos serviços de integração do Hyper-V. Esse tipo de adaptador de rede mais rápido e é a opção recomendada, a menos que você precise inicializar na rede ou esteja executando um sistema operacional convidado sem suporte. O driver necessário é fornecido somente para sistemas operacionais convidados com suporte. Observe que no Gerenciador do Hyper-V e nos cmdlets de rede, esse tipo é chamado apenas de adaptador de rede.  
  
-   **Adaptador de rede herdado** – disponível somente em máquinas virtuais de geração 1. Emula um adaptador Ethernet rápido PCI baseado em Intel 21140 e pode ser usado para inicializar uma rede para que você possa instalar um sistema operacional de um serviço como os serviços de implantação do Windows.  
  
## <a name="hyper-v-networking-and-related-technologies"></a>Rede do Hyper-V e tecnologias relacionadas  
As versões recentes do Windows Server introduziram melhorias que oferecem mais opções para configurar a rede para o Hyper-V. Por exemplo, o Windows Server 2012 introduziu o suporte para rede convergida. Isso permite rotear o tráfego de rede por meio de um comutador virtual externo. O Windows Server 2016 baseia-se nele permitindo o acesso remoto direto à memória (RDMA) nos adaptadores de rede associados a um comutador virtual Hyper-V. Você pode usar essa configuração com ou sem o switch Embedded Integration (SET). Para obter detalhes,consulte [Acesso remoto direto à memória &#40;Access&#41; RDMA e troca de equipe inserida &#40;Set&#41; ](../../hyper-v-virtual-switch/RDMA-and-Switch-Embedded-Teaming.md)  
  
Alguns recursos dependem de configurações de rede específicas ou têm melhor desempenho em determinadas configurações. Considere isso ao planejar ou atualizar sua infraestrutura de rede.  
  
**Clustering de failover** – é uma prática recomendada isolar o tráfego de cluster e usar a QoS (qualidade de serviço) do Hyper-V no comutador virtual. Para obter detalhes, consulte [recomendações de rede para um cluster do Hyper-V](https://technet.microsoft.com/library/dn550728.aspx)  
  
**Migração dinâmica** – use as opções de desempenho para reduzir o uso da rede e da CPU e o tempo necessário para concluir uma migração ao vivo. Para obter instruções, consulte [Configurar hosts para migração ao vivo sem clustering de failover](../deploy/set-up-hosts-for-live-migration-without-failover-clustering.md).  
  
**Espaços de armazenamento diretos** -esse recurso depende do protocolo de rede SMB 3.0 e do RDMA. Para obter detalhes, consulte [espaços de armazenamento diretos no Windows Server 2016](../../../storage/storage-spaces/storage-spaces-direct-overview.md).