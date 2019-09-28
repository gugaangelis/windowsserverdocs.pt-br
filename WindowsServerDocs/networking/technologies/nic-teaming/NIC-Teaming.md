---
title: Agrupamento NIC
description: Neste tópico, fornecemos uma visão geral do agrupamento NIC (placa de interface de rede) no Windows Server 2016. O agrupamento NIC permite que você agrupe entre um e 32 adaptadores de rede Ethernet físicos em um ou mais adaptadores de rede virtual baseados em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.
manager: dougkim
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.date: 09/10/2018
ms.openlocfilehash: 2356de674bfc6e57c9444136b1244934464a2d02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71396502"
---
# <a name="nic-teaming"></a>Agrupamento NIC

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Neste tópico, fornecemos uma visão geral do agrupamento NIC (placa de interface de rede) no Windows Server 2016. O agrupamento NIC permite que você agrupe entre um e 32 adaptadores de rede Ethernet físicos em um ou mais adaptadores de rede virtual baseados em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.  
  
> [!IMPORTANT]
> Você deve instalar adaptadores de rede de membros da equipe NIC no mesmo computador host físico. 

> [!TIP]  
> Uma equipe NIC que contém apenas um adaptador de rede não pode fornecer balanceamento de carga e failover. No entanto, com um adaptador de rede, você pode usar agrupamento NIC para separação de tráfego de rede quando você também estiver usando redes locais virtuais (VLANs).  
  
Quando você configura adaptadores de rede em uma equipe NIC, eles se conectam à solução de agrupamento NIC Common Core, que apresenta um ou mais adaptadores virtuais (também chamados de NICs de equipe [tNICs] ou interfaces de equipe) ao sistema operacional. 

Como o Windows Server 2016 dá suporte a até 32 interfaces de equipe por equipe, há uma variedade de algoritmos que distribuem o tráfego de saída (carga) entre as NICs.  A ilustração a seguir descreve uma equipe NIC com vários tNICs.  
  
![Equipe NIC com vários tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
Além disso, você pode conectar suas NICs agrupadas ao mesmo comutador ou a diferentes opções. Se você conectar NICs a diferentes comutadores, ambos os comutadores deverão estar na mesma sub-rede.  
  
## <a name="availability"></a>Disponibilidade  
O agrupamento NIC está disponível em todas as versões do Windows Server 2016. Você pode usar uma variedade de ferramentas para gerenciar o agrupamento NIC de computadores que executam um sistema operacional cliente, como: • cmdlets do Windows PowerShell • Área de Trabalho Remota • Ferramentas de Administração de Servidor Remoto  
  
## <a name="supported-and-unsupported-nics"></a>NICs com e sem suporte   
Você pode usar qualquer NIC Ethernet que tenha aprovado o teste de logotipo e de qualificação de hardware do Windows (testes de WHQL) em uma equipe NIC no Windows Server 2016.  
  
Você não pode inserir as seguintes NICs em uma equipe NIC:
  
-   Adaptadores de rede virtual do Hyper-V que são portas do comutador virtual Hyper-V expostas como NICs na partição de host.  
  
    > [!IMPORTANT]  
    > Não coloque as NICs virtuais do Hyper-V expostas na vNICs (partição de host) em uma equipe. Não há suporte para o agrupamento de vNICs dentro da partição de host em nenhuma configuração. Tentativas de vNICs de equipe podem causar uma perda completa de comunicação se ocorrerem falhas de rede.  
  
-   Adaptador de rede de depuração do kernel (KDNIC).  
  
-   NICs usadas para inicialização de rede.  
  
-   NICs que usam tecnologias diferentes de Ethernet, como WWAN, WLAN/Wi-Fi, Bluetooth e InfiniBand, incluindo NICs do protocolo Internet sobre InfiniBand (IPoIB).  
  
## <a name="compatibility"></a>Compatibilidade  
O agrupamento NIC é compatível com todas as tecnologias de rede no Windows Server 2016 com as seguintes exceções.  
  
-   **Sr-IOV (virtualização de e/s de raiz única)** . Para SR-IOV, os dados são entregues diretamente à NIC sem passá-los por meio da pilha de rede (no sistema operacional do host, no caso de virtualização). Portanto, não é possível que a equipe NIC Inspecione ou Redirecione os dados para outro caminho na equipe.  
  
-   **QoS (qualidade de serviço) do host nativo**. Quando você define políticas de QoS em um sistema nativo ou de host, e essas políticas chamam limitações de largura de banda mínima, a taxa de transferência geral para uma equipe NIC é menor do que seria sem as políticas de largura de banda em vigor.  
  
-   **Chimney TCP**. Não há suporte para TCP Chimney com agrupamento NIC porque o Chimney TCP descarrega toda a pilha de rede diretamente para a NIC.  
  
-   **autenticação 802.1 x**. Você não deve usar a autenticação 802.1 X com agrupamento NIC, pois algumas opções não permitem a configuração da autenticação 802.1 X e do agrupamento NIC na mesma porta.  
  
Para saber mais sobre como usar o agrupamento NIC em máquinas virtuais (VMs) executadas em um host Hyper-V, confira [criar uma nova equipe NIC em um computador host ou VM](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md).
  
## <a name="virtual-machine-queues-vmqs"></a>Filas de máquina virtual (VMQs)  

VMQs é um recurso de NIC que aloca uma fila para cada VM.  Sempre que você tiver habilitado o Hyper-V; Você também deve habilitar a VMQ. No Windows Server 2016, VMQs use o comutador NIC vPorts com uma única fila atribuída ao vPort para fornecer a mesma funcionalidade. 

Dependendo do modo de configuração do comutador e do algoritmo de distribuição de carga, o agrupamento NIC apresenta o menor número de filas disponíveis e com suporte por qualquer adaptador na equipe (modo min-Queues) ou o número total de filas disponíveis em todas as equipes Membros (modo de soma de filas).  

Se a equipe estiver no modo de agrupamento independente de comutador e você definir a distribuição de carga para o modo de porta do Hyper-V ou modo dinâmico, o número de filas relatadas será a soma de todas as filas disponíveis nos membros da equipe (modo de soma das filas). Caso contrário, o número de filas relatadas é o menor número de filas suportadas por qualquer membro da equipe (modo mín. de filas).

Veja o porquê:  
  
-   Quando a equipe independente de comutador estiver no modo de porta do Hyper-V ou no modo dinâmico, o tráfego de entrada para uma VM (porta do comutador) do Hyper-V sempre chegará ao mesmo membro da equipe. O host pode prever/controlar qual membro recebe o tráfego para uma VM específica, de modo que o agrupamento NIC possa ser mais elaborado sobre quais filas de VMQ alocar em um determinado membro da equipe. O agrupamento NIC, trabalhando com a opção Hyper-V, define a VMQ para uma VM em um membro da equipe precisamente e sabe que o tráfego de entrada atinge essa fila.  
  
-   Quando a equipe está em qualquer modo dependente de switch (agrupamento estático ou Agrupamento de LACP), o comutador ao qual a equipe está conectada controla a distribuição de tráfego de entrada. O software de agrupamento NIC do host não pode prever qual membro da equipe Obtém o tráfego de entrada para uma VM e pode ser que o comutador distribua o tráfego para uma VM em todos os membros da equipe. Como resultado do software de agrupamento NIC, trabalhar com a opção Hyper-V, programa uma fila para a VM em cada membro da equipe, não apenas um membro da equipe.  
  
-   Quando a equipe está no modo independente de comutador e usa o balanceamento de carga de hash de endereço, o tráfego de entrada sempre entra em uma NIC (o membro da equipe principal) – tudo isso em apenas um membro da equipe. Como outros membros da equipe não estão lidando com o tráfego de entrada, eles são programados com as mesmas filas que o membro primário para que, se o membro primário falhar, qualquer outro membro da equipe possa ser usado para pegar o tráfego de entrada e as filas já estiverem em vigor.  

- A maioria das NICs tem filas usadas para o RSS (modo de recebimento) ou a VMQ, mas não ao mesmo tempo. Algumas configurações de VMQ parecem ser configurações para filas RSS, mas são configurações nas filas genéricas que o RSS e a VMQ usam, dependendo de qual recurso está atualmente em uso. Cada NIC tem, em suas propriedades avançadas, valores para * RssBaseProcNumber e \*MaxRssProcessors. A seguir estão algumas configurações de VMQ que fornecem melhor desempenho do sistema.  
  
-   O ideal é que cada NIC tenha o * RssBaseProcNumber definido como um número par maior ou igual a dois (2). O primeiro processador físico, o núcleo 0 (processadores lógicos 0 e 1), normalmente faz a maior parte do processamento do sistema, de modo que o processamento da rede deve ser afastado desse processador físico. Algumas arquiteturas de computador não têm dois processadores lógicos por processador físico, portanto, para esses computadores, o processador base deve ser maior ou igual a 1. Em caso de dúvida, suponha que o host esteja usando um processador lógico 2 por arquitetura de processador físico.  
  
-   Se a equipe estiver no modo de soma das filas, os processadores de membros da equipe não devem ser sobrepostos. Por exemplo, em um host de 4 núcleos (8 processadores lógicos) com uma equipe de 2 NICs 10 Gbps, você pode definir o primeiro para usar o processador base 2 e usar 4 núcleos; o segundo seria definido para usar o processador base 6 e usar dois núcleos.  
  
-   Se a equipe estiver no modo de filas mínimas, os conjuntos de processadores usados pelos membros da equipe deverão ser idênticos.  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Virtualização de Rede Hyper-V (HNV)  
O agrupamento NIC é totalmente compatível com a HNV (virtualização de rede do Hyper-V).  O sistema de gerenciamento de HNV fornece informações para o driver de agrupamento NIC que permite que o agrupamento NIC distribua a carga de forma a otimizar o tráfego HNV.  
  
## <a name="live-migration"></a>Migração ao vivo  
O agrupamento NIC em VMs não afeta Migração ao Vivo. As mesmas regras existem para Migração ao Vivo se está ou não Configurando o agrupamento NIC na VM.  


## <a name="virtual-local-area-networks-vlans"></a>VLANs (redes locais virtuais)
Quando você usa o agrupamento NIC, a criação de várias interfaces de equipe permite que um host se conecte a VLANs diferentes ao mesmo tempo. Configure seu ambiente usando as seguintes diretrizes:
  
- Antes de habilitar o agrupamento NIC, configure as portas de comutador físico conectadas ao host de agrupamento para usar o modo de tronco (promíscuo). O comutador físico deve passar todo o tráfego para o host para filtragem sem modificar o tráfego.  

- Não configure filtros de VLAN nas NICs usando as configurações de propriedades avançadas de NIC. Permita que o software de agrupamento NIC ou o comutador virtual Hyper-V (se presente) execute a filtragem de VLAN.  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>Usar VLANs com agrupamento NIC em uma VM  
Quando uma equipe se conecta a um comutador virtual do Hyper-V, toda a diferenciação de VLAN deve ser feita no comutador virtual do Hyper-V em vez de no agrupamento NIC.  

Planeje usar VLANs em uma VM configurada com uma equipe NIC usando as seguintes diretrizes:
  
-   O método preferencial de dar suporte a várias VLANs em uma VM é configurar a VM com várias portas no comutador virtual do Hyper-V e associar cada porta a uma VLAN. Nunca faça uma equipe dessas portas na VM porque isso causa problemas de comunicação de rede.  

-   Se a VM tiver várias funções virtuais de SR-IOV (VFs), verifique se elas estão na mesma VLAN antes de agrupá-las na VM. É facilmente possível configurar o VFs diferente para estar em VLANs diferentes e fazer isso causa problemas de comunicação de rede.  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>Gerenciar adaptadores de rede e VLANs 
Se você precisar ter mais de uma VLAN exposta em um sistema operacional convidado, considere renomear as interfaces Ethernet para esclarecer a VLAN atribuída à interface. Por exemplo, se você associar a interface **Ethernet** com a VLAN 12 e a interface **Ethernet 2** com VLAN 48, renomeie a interface Ethernet para **EthernetVLAN12** e a outra para **EthernetVLAN48**. 

Renomeie interfaces usando o comando do Windows PowerShell **Rename-netadapter** ou executando o seguinte procedimento:
  
1.  Em Gerenciador do Servidor, em **Propriedades** do adaptador de rede que você deseja renomear, clique no link à direita do nome do adaptador de rede. 
  
2.  Clique com o botão direito do mouse no adaptador de rede que você deseja renomear e selecione **renomear**.  
  
3.  Digite o novo nome para o adaptador de rede e pressione ENTER.  


## <a name="virtual-machines-vms"></a>Máquinas virtuais (VMs)

Se você quiser usar o agrupamento NIC em uma VM, você deve conectar os adaptadores de rede virtual na VM somente a comutadores virtuais Hyper-V externos. Isso permite que a VM mantenha a conectividade de rede mesmo na circunstância quando um dos adaptadores de rede física conectados a um comutador virtual falha ou é desconectado. Os adaptadores de rede virtual conectados a comutadores virtuais do Hyper-V internos ou privados não são capazes de se conectar ao comutador quando estão em uma equipe e a rede falha para a VM.  
  
O agrupamento NIC no Windows Server 2016 dá suporte a equipes com dois membros em VMs. Você pode criar equipes maiores, mas não há suporte para equipes maiores. Cada membro da equipe deve se conectar a um comutador virtual Hyper-V externo diferente e as interfaces de rede da VM devem ser configuradas para permitir o agrupamento.

  
Se você estiver configurando uma equipe NIC em uma VM, deverá selecionar um **modo** de agrupamento _independente de comutador_ e um **modo de balanceamento de carga** de hash de _endereço_.   
  
  
## <a name="sr-iov-capable-network-adapters"></a>Adaptadores de rede compatíveis com SR-IOV  
Uma equipe NIC no ou no host Hyper-V não pode proteger o tráfego de SR-IOV porque ele não passa pelo comutador do Hyper-V.  Com a opção de agrupamento NIC da VM, você pode configurar dois comutadores virtuais Hyper-V externos, cada um conectado à sua própria NIC compatível com SR-IOV.  
  
![Agrupamento NIC com adaptadores de rede compatíveis com SR-IOV](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
Cada VM pode ter uma função virtual (FV) de uma ou ambas as NICs de SR-IOV e, no caso de uma desconexão de NIC, o failover do FV primário para o adaptador de backup (FV). Como alternativa, a VM pode ter um VF de uma NIC e um vmNIC não VF conectado a outro comutador virtual. Se a NIC associada ao FV for desconectada, o tráfego poderá fazer failover para o outro comutador sem perda de conectividade.  
  
Como o failover entre NICs em uma VM pode resultar no tráfego enviado com o endereço MAC do outro vmNIC, cada porta do comutador virtual Hyper-V associada a uma VM usando o agrupamento NIC deve ser definida para permitir o agrupamento. 


## <a name="related-topics"></a>Tópicos relacionados

- [Uso e gerenciamento de endereço MAC de agrupamento NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Quando você configura uma equipe NIC com o modo independente de comutador e endereço hash ou distribuição dinâmica de carga, a equipe usa o endereço MAC (controle de acesso à mídia) do membro da equipe NIC primário no tráfego de saída. O membro da equipe NIC primário é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.

- [Configurações de agrupamento NIC](nic-teaming-settings.md): Neste tópico, fornecemos uma visão geral das propriedades da equipe da NIC, como os modos de agrupamento e balanceamento de carga. Também fornecemos detalhes sobre a configuração do adaptador em espera e a propriedade da interface da equipe principal. Se você tiver pelo menos dois adaptadores de rede em uma equipe NIC, não será necessário designar um adaptador em espera para tolerância a falhas.
  
- [Crie uma nova equipe NIC em um computador host ou VM](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): Neste tópico, você cria uma nova equipe NIC em um computador host ou em uma VM (máquina virtual) do Hyper-V que executa o Windows Server 2016.

- [Solução de problemas de agrupamento NIC](Troubleshooting-NIC-Teaming.md): Neste tópico, discutiremos maneiras de solucionar problemas de agrupamento de NIC, como hardware, comutador físico de valores e desabilitar ou habilitar adaptadores de rede usando o Windows PowerShell. 
 
