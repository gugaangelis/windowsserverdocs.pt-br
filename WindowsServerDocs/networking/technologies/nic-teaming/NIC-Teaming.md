---
title: Agrupamento NIC
description: Neste tópico, nós fornecemos a você uma visão geral do agrupamento de placa de Interface de rede (NIC) no Windows Server 2016. Agrupamento NIC permite que você agrupe entre um e 32 adaptadores de rede Ethernet física em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: abded6f3-5708-4e35-9a9e-890e81924fec
ms.author: pashort
author: shortpatti
ms.date: 09/10/2018
ms.openlocfilehash: cf58956ead8e8a47b8ec6d189bf23e5c576d5f15
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812187"
---
# <a name="nic-teaming"></a>Agrupamento NIC

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Neste tópico, nós fornecemos a você uma visão geral do agrupamento de placa de Interface de rede (NIC) no Windows Server 2016. Agrupamento NIC permite que você agrupe entre um e 32 adaptadores de rede Ethernet física em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.  
  
> [!IMPORTANT]
> Você deve instalar adaptadores de rede de membro de agrupamento NIC no mesmo computador host físico. 

> [!TIP]  
> Um agrupamento NIC que contém apenas um adaptador de rede não pode fornecer balanceamento de carga e failover. No entanto, com um adaptador de rede, você pode usar o agrupamento NIC para separar o tráfego de rede quando você também estiver usando redes locais virtuais (VLANs).  
  
Quando você configura os adaptadores de rede em um agrupamento NIC, eles se conectam na NIC agrupamento solução comum principal, que, em seguida, apresenta um ou mais adaptadores virtuais (também chamados de interfaces de equipe ou equipe NICs [tNICs]) para o sistema operacional. 

Uma vez que o Windows Server 2016 dá suporte a até 32 adaptadores de equipe por equipe, há uma variedade de algoritmos que distribui o tráfego de saída (carga) entre as NICs.  A ilustração a seguir ilustra uma equipe NIC com vários tNICs.  
  
![Agrupamento NIC com vários tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
Além disso, você pode conectar os NICs agrupados ao mesmo comutador ou comutadores diferentes. Se você conectar as NICs a diferentes comutadores, ambas as opções devem ser na mesma sub-rede.  
  
## <a name="availability"></a>Disponibilidade  
Agrupamento NIC está disponível em todas as versões do Windows Server 2016. Você pode usar uma variedade de ferramentas para gerenciar o agrupamento NIC de computadores que executam um sistema operacional cliente, tais como: • • de cmdlets do Windows PowerShell área de trabalho remota • ferramentas de administração de servidor remoto  
  
## <a name="supported-and-unsupported-nics"></a>NICs compatíveis e sem suportadas   
Você pode usar qualquer NIC Ethernet que passou a qualificação de Hardware do Windows e o logotipo teste (testes do WHQL) em uma equipe de NIC no Windows Server 2016.  
  
Você não pode colocar as NICs a seguir em uma equipe NIC:
  
-   Adaptadores de rede virtual do Hyper-V que são expostas como NICs na partição de host de portas de comutador Virtual Hyper-V.  
  
    > [!IMPORTANT]  
    > Não coloque NICs virtuais do Hyper-V, expostas na partição host (vNICs) em uma equipe. Não há suporte para o agrupamento de vNICs dentro da partição de host em qualquer configuração. Tentativas de vNICs equipe podem causar uma perda completa de comunicação se ocorrerem falhas de rede.  
  
-   Adaptador de rede de depuração do kernel (KDNIC).  
  
-   NICs são usadas para inicialização de rede.  
  
-   NICs que usam tecnologias que não seja de Ethernet, como WWAN, WLAN/Wi-Fi, Bluetooth e Infiniband, incluindo o protocolo IP sobre Infiniband (IPoIB) NICs.  
  
## <a name="compatibility"></a>Compatibilidade  
Agrupamento NIC é compatível com todas as tecnologias de rede no Windows Server 2016 com as seguintes exceções.  
  
-   **Virtualização de e/s de raiz única (SR-IOV)** . Para SR-IOV, os dados são entregues diretamente para a placa de rede sem passá-los pela pilha da rede (no sistema operacional do host, no caso de virtualização). Portanto, não é possível que o agrupamento NIC inspecionar ou redirecionar os dados para outro caminho na equipe.  
  
-   **Qualidade de serviço (QoS) de host nativo**. Quando você define políticas de QoS no sistema de host ou um nativo e essas políticas invocam limitações de largura de banda mínima, a taxa de transferência geral para uma equipe NIC for menor do que seria sem as políticas de largura de banda em vigor.  
  
-   **TCP Chimney**. TCP Chimney não é compatível com o agrupamento NIC porque TCP Chimney descarrega a pilha inteira de rede diretamente à NIC.  
  
-   **802.1 autenticação de x**. Você não deve usar a autenticação 802.1 X com o agrupamento NIC porque algumas opções não é possível fazer a configuração de autenticação 802.1 X e o agrupamento NIC na mesma porta.  
  
Para saber mais sobre como usar o agrupamento NIC em máquinas virtuais (VMs) que são executados em um host Hyper-V, consulte [criar um novo agrupamento NIC em uma VM ou computador host](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md).
  
## <a name="virtual-machine-queues-vmqs"></a>Filas de máquina virtual (VMQs)  

VMQs é um recurso NIC que aloca uma fila para cada VM.  Sempre que tiver o Hyper-V habilitado; Você também deve habilitar VMQ. No Windows Server 2016, VMQs usam vPorts NIC Switch com uma única fila atribuída para o vPort para fornecer a mesma funcionalidade. 

Dependendo do modo de configuração de comutador e o algoritmo de distribuição de carga, o agrupamento NIC apresenta o menor número de filas disponíveis e com suporte por qualquer adaptador da equipe (modo de Min-filas) ou o número total de filas disponíveis em todos os team membros (modo de soma de filas).  

Se a equipe está no modo de agrupamento independentes de comutador e você definir a distribuição de carga para o modo de porta Hyper-V ou no modo dinâmico, o número de filas relatado é a soma de todas as filas disponíveis de membros da equipe (modo de soma de filas). Caso contrário, o número de filas relatado é o menor número de filas com suporte por qualquer membro da equipe (modo de Min-filas).

Eis o porquê:  
  
-   Quando a equipe do comutador independente está no modo de porta Hyper-V ou dinâmico o tráfego de entrada para uma porta de comutador do Hyper-V (VM) sempre são recebidos no mesmo membro da equipe. O host pode prever/controle qual membro recebe o tráfego para uma determinada VM, para que o agrupamento NIC pode ser mais profunda sobre quais filas VMQ para alocar em um membro específico da equipe. O agrupamento NIC, trabalhando com o comutador do Hyper-V, define a VMQ para uma VM em exatamente um membro da equipe e saber que o tráfego atinge essa fila.  
  
-   Quando a equipe estiver em qualquer modo dependentes de switch (agrupamento estático ou LACP teaming), o que a equipe está conectada ao comutador controla a distribuição do tráfego de entrada. Software de agrupamento NIC do host não pode prever qual equipe member obtém o tráfego de entrada para uma máquina virtual e pode ser que o comutador distribui o tráfego para uma VM em todos os membros da equipe. Como resultado do software de agrupamento NIC, trabalhando com o comutador do Hyper-V, programas de uma fila para a máquina virtual em cada membro da equipe, não apenas um membro da equipe do.  
  
-   Quando a equipe está no modo independente do comutador e o hash de endereço usa o balanceamento de carga, o tráfego de entrada sempre entra em uma NIC (o membro da equipe principal) - tudo isso em apenas um membro da equipe. Uma vez que outros membros da equipe não estão lidando com o tráfego de entrada, eles obterem programados com filas mesmas que o membro primário para que se o membro primário falhar, qualquer outro membro da equipe pode ser usado para acompanhar o tráfego de entrada e as filas já estão em vigor.  

- A maioria das NICs têm filas usadas para RSS Receive Side Scaling () ou VMQ, mas não ao mesmo tempo. Algumas configurações de VMQ parecem ser as configurações para as filas RSS, mas são configurações nas filas genéricas que uso RSS e VMQ, dependendo de qual recurso está atualmente em uso. Cada NIC tem, em suas propriedades avançadas, valores para * RssBaseProcNumber e \*MaxRssProcessors. A seguir estão algumas configurações de VMQ que fornecem melhor desempenho do sistema.  
  
-   O ideal é que cada NIC deve ter o * RssBaseProcNumber definido como um número par, maior que ou igual a dois (2). O primeiro processador físico, Core 0 (processadores lógicos 0 e 1), normalmente faz a maioria do processamento do sistema para que o processamento de rede deve conduzir para longe desse processador físico. Algumas arquiteturas de computador não tem dois processadores lógicos por processador físico, portanto, para essas máquinas, o processador de base deve ser maior que ou igual a 1. Se estiver em dúvida assumir que o host está usando um processador lógico 2 por arquitetura de processador físico.  
  
-   Se a equipe estiver no modo de soma de filas processadores de membros da equipe devem ser não sobrepostos. Por exemplo, em um host de 4 núcleos (8 processadores lógicos) com uma equipe de 2 NICs de 10 Gbps, você poderia definir a primeira para usar o processador de 2 de base e usar 4 núcleos; o segundo seria definido para usar o processador de base 6 e usar 2 núcleos.  
  
-   Se a equipe estiver no modo de Min-filas, os conjuntos de processador usados por membros da equipe devem ser idênticos.  

  
## <a name="hyper-v-network-virtualization-hnv"></a>Virtualização de Rede Hyper-V (HNV)  
Agrupamento NIC é totalmente compatível com virtualização de rede Hyper-V (HNV).  O sistema de gerenciamento de HNV fornece informações para o driver de agrupamento NIC que permite que o agrupamento NIC distribuir a carga de uma maneira que otimiza o tráfego da HNV.  
  
## <a name="live-migration"></a>Migração ao vivo  
Agrupamento NIC em máquinas virtuais não afeta a migração ao vivo. As mesmas regras existem para migração ao vivo ou não configurar o agrupamento NIC na VM.  


## <a name="virtual-local-area-networks-vlans"></a>Redes locais virtuais (VLANs)
Quando você usa o agrupamento NIC, criação de várias interfaces de equipe permite que um host para se conectar a VLANs diferentes ao mesmo tempo. Configure seu ambiente usando as diretrizes a seguir:
  
- Antes de habilitar o agrupamento NIC, configure as portas de comutador físico conectadas ao host de agrupamento para usar o modo de tronco (promíscuo). O comutador físico deve passar todo o tráfego para o host para filtragem sem modificar o tráfego.  

- Não configure filtros VLAN nas NICs usando a NIC de configurações de propriedades avançada. Permitir que o software de agrupamento NIC ou o comutador Virtual do Hyper-V (se houver) realizar a filtragem de VLAN.  
  
### <a name="use-vlans-with-nic-teaming-in-a-vm"></a>Usar VLANs com o agrupamento NIC em uma VM  
Quando uma equipe se conecta a um comutador Virtual do Hyper-V, todos os segregação da VLAN deve ser feita no comutador Virtual Hyper-V em vez de em agrupamento NIC.  

Planeje usar VLANs em uma VM configurada com uma equipe NIC usando as seguintes diretrizes:
  
-   É o método preferencial de dar suporte a várias VLANs em uma VM configurar a VM com várias portas no comutador Virtual Hyper-V e associar cada porta de uma VLAN. Nunca equipe essas portas na VM, porque isso fizer problemas de comunicação de rede.  

-   Se a VM tiver várias funções virtuais SR-IOV (VFs), certifique-se de que eles estão na mesma VLAN antes de agrupamento-los na VM. É possível facilmente configurar VFs diferentes para ser em VLANs diferentes, e isso fizer problemas de comunicação de rede.  
 
  
### <a name="manage-network-interfaces-and-vlans"></a>Gerenciar adaptadores de rede e VLANs 
Se você precisar ter mais de uma VLAN exposto em um sistema operacional convidado, considere a possibilidade de renomear as interfaces de Ethernet para esclarecer a VLAN atribuído à interface. Por exemplo, se você associar **Ethernet** interface com 12 VLAN e a **Ethernet 2** interface com 48 de VLAN, renomeie a interface Ethernet para **EthernetVLAN12** e o para outros **EthernetVLAN48**. 

Renomear interfaces usando o comando do Windows PowerShell **Rename-NetAdapter** ou executando o procedimento a seguir:
  
1.  No Gerenciador do servidor, no **propriedades** para o adaptador de rede que você deseja renomear, clique no link à direita do nome do adaptador de rede. 
  
2.  O adaptador de rede que você deseja renomear e clique com o botão direito **Renomear**.  
  
3.  Digite o novo nome para o adaptador de rede e pressione ENTER.  


## <a name="virtual-machines-vms"></a>Máquinas virtuais (VMs)

Se você quiser usar o agrupamento NIC em uma VM, você deve conectar os adaptadores de rede virtual na VM para Hyper-V comutadores virtuais externos somente. Isso permite que a VM manter a conectividade de rede, mesmo na circunstância quando um dos adaptadores de rede física conectado a um comutador virtual falhar ou for desconectado. Adaptadores de rede virtuais conectados a comutadores virtuais interno ou privado de Hyper-V não são capazes de se conectar ao comutador quando eles estão em uma equipe e falha de rede para a VM.  
  
Agrupamento NIC no Windows Server 2016 dá suporte a equipes com dois membros em máquinas virtuais. Você pode criar equipes maiores, mas não há suporte para equipes maiores. Cada membro da equipe deve se conectar a um comutador de Virtual de Hyper-V externo diferente e interfaces de rede da VM devem ser configurados para permitir o agrupamento.

  
Se você estiver configurando um agrupamento NIC em uma VM, você deve selecionar um **modo de agrupamento** dos _independente de comutador_ e um **modo de balanceamento de carga** de _deHashdeendereço_.   
  
  
## <a name="sr-iov-capable-network-adapters"></a>Adaptadores de rede compatíveis com SR-IOV  
Um agrupamento NIC no ou sob o host do Hyper-V não pode proteger o tráfego de SR-IOV, porque não passa pelo comutador do Hyper-V.  Com a opção de agrupamento de NIC de VM, você pode configurar dois comutadores de Virtual do Hyper-V externo, cada uma conectada a própria placa de rede compatíveis com SR-IOV.  
  
![NIC Teaming com adaptadores de rede compatíveis com SR-IOV](../../media/NIC-Teaming-in-Virtual-Machines--VMs-/nict_in_vm.jpg)  
  
Cada VM pode ter uma função de virtual (FV) de uma ou ambas as NICs de SR-IOV e, em caso de uma desconexão da NIC, o failover de VF primário para o adaptador de backup (FV). Como alternativa, a VM pode ter um VF de uma NIC e uma vmNIC não VF conectados a outro comutador virtual. Se a NIC associada à VF for desconectado, o tráfego pode fazer failover para outro comutador sem perda de conectividade.  
  
Porque o failover entre NICs em uma VM pode resultar em tráfego enviado com o endereço MAC de outro vmNIC, cada porta de comutador Virtual Hyper-V associada a uma VM usando o agrupamento NIC deve ser definida para permitir o agrupamento. 


## <a name="related-topics"></a>Tópicos relacionados

- [Gerenciamento e uso do endereço MAC de agrupamento NIC](NIC-Teaming-MAC-Address-Use-and-Management.md): Quando você configura uma equipe NIC com a opção de modo independente e hash de endereço ou distribuição de carga dinâmico, a equipe usa que o acesso à mídia (MAC) de endereço do membro da equipe de NIC primário no tráfego de saída de controle. O membro da equipe de NIC primário é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.

- [Configurações de agrupamento NIC](nic-teaming-settings.md): Neste tópico, podemos lhe dar uma visão geral das propriedades de agrupamento NIC, como agrupamento e modos de balanceamento de carga. Nós também fornecemos a você detalhes sobre a configuração do adaptador em espera e a propriedade de interface de equipe principal. Se você tiver pelo menos dois adaptadores de rede em um agrupamento NIC, você não precisa designar um adaptador de modo de espera para tolerância a falhas.
  
- [Criar um novo agrupamento NIC em uma VM ou computador host](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): Neste tópico, você cria um novo agrupamento NIC em um computador host ou em uma máquina virtual do Hyper-V (VM) executando o Windows Server 2016.

- [Solução de problemas de agrupamento NIC](Troubleshooting-NIC-Teaming.md): Neste tópico, discutiremos maneiras de solucionar problemas de agrupamento NIC, como hardware, firmas de comutador físico e desabilitando ou habilitando adaptadores de rede usando o Windows PowerShell. 
 
