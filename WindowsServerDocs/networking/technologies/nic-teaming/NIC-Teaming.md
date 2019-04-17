---
title: Agrupamento de NIC
description: Este tópico fornece uma visão geral de agrupamento de placa de rede (NIC) no Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 142f56153187368effdb802c0c1b50359fffc36a
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming"></a>Agrupamento de NIC

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Este tópico fornece uma visão geral de agrupamento de placa de rede (NIC) no Windows Server 2016.

> [!NOTE]  
> Além de neste tópico, o seguinte conteúdo de agrupamento de placa de rede está disponível.  
>   
> - [NIC agrupamento em máquinas virtuais & #40; VMs & #41;](nict-vms.md)
> - [Agrupamento de NIC e redes locais virtuais & #40; VLANs & #41;](nict-and-vlans.md)
> - [Agrupamento de gerenciamento e o uso de endereço MAC do NIC](NIC-Teaming-MAC-Address-Use-and-Management.md)
> - [Solução de problemas NIC agrupamento](Troubleshooting-NIC-Teaming.md) 
> - [Criar uma nova equipe de placa de rede em um computador Host ou VM](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)
> - [NIC agrupamento Cmdlets (NetLBFO) no Windows PowerShell](https://technet.microsoft.com/library/jj130849.aspx)
> - Download de galeria do TechNet: [Windows Server 2016 NIC e alternar agrupamento Embedded guia do usuário](https://gallery.technet.microsoft.com/Windows-Server-2016-839cb607?redir=0)
  
## <a name="bkmk_over"></a>Visão geral de agrupamento de NIC  
Agrupamento de placa de rede permite que você agrupe entre 1 e 2 trinta físicos adaptadores de rede Ethernet em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual fornecem desempenho rápido e tolerância no caso de uma falha de adaptador de rede.  
  
Todos os adaptadores de rede de membro de equipe de NIC devem ser instalados no mesmo computador host físico deve ser colocado em uma equipe.  
  
> [!NOTE]  
> Uma equipe NIC que contém apenas um adaptador de rede não pode fornecer balanceamento de carga e failover; No entanto com um adaptador de rede, você pode usar NIC agrupamento de separação de tráfego de rede quando você também usa as redes locais virtuais (VLANs).  
  
Quando você configurar adaptadores de rede em uma equipe NIC, eles são conectados no NIC agrupamento solução núcleo comum, que, em seguida, apresenta um ou mais adaptadores virtuais (também chamados de equipe NICs [tNICs] ou interfaces de equipe) para o sistema operacional. Windows Server 2016 dá suporte a até 32 interfaces de equipe por equipe. Há uma variedade de algoritmos de distribuir o tráfego de saída (carregar) entre as NICs.  
  
A ilustração a seguir mostra uma equipe de NIC com vários tNICs.  
  
![Equipe de NIC com vários tNICs](../../media/NIC-Teaming/nict_overview.jpg)  
  
Além disso, você pode conectar seu NICs emparelhados ao mesmo switch ou opções diferentes. Se você conectar NICs para diferentes opções, ambas as opções devem ser na mesma sub-rede.  
  
## <a name="bkmk_avail"></a>Disponibilidade de agrupamento de NIC  
Agrupamento de placa de rede está disponível em todas as versões do Windows Server 2016. Além disso, você pode usar comandos do Windows PowerShell, área de trabalho remota e ferramentas de administração de servidor remoto para gerenciar o agrupamento de NIC de computadores que executam um sistema operacional do cliente no qual as ferramentas são compatíveis.  
  
## <a name="bkmk_nics"></a>NICs compatíveis e sem suportadas para o agrupamento de NIC  
Você pode usar qualquer NIC Ethernet que tenha aprovado no teste de qualificação de Hardware do Windows e o logotipo (testes WHQL) em uma equipe de placa de rede no Windows Server 2016.  
  
As seguintes NICs não podem ser colocadas em uma equipe de placa de rede.  
  
-   Adaptadores de rede virtual Hyper-V que são expostas como NICs na partição do host de portas de comutador Virtual Hyper-V.  
  
    > [!IMPORTANT]  
    > Hyper-V NICs virtuais que são expostas na partição do host (vNICs) não devem ser colocadas em uma equipe. Não há suporte para o agrupamento de vNICs dentro de partição do host em qualquer configuração ou uma combinação. Tentativas de equipe vNICs podem causar perda de comunicação completa caso ocorram falhas de rede.  
  
-   O kernel depurar adaptador de rede (KDNIC).  
  
-   NICs que estão sendo usadas para a inicialização de rede.  
  
-   NICs que usam as tecnologias que não sejam Ethernet, como WWAN, WLAN/Wi-Fi, Bluetooth e Infiniband, incluindo o protocolo de Internet NICs Infiniband (IPoIB).  
  
## <a name="bkmk_compat"></a>Compatibilidade de agrupamento de NIC  
Agrupamento de NIC é compatível com todas as tecnologias de rede no Windows Server 2016 com as seguintes exceções.  
  
-   **Virtualização de e/s de raiz única (SR-IOV)**. Para SR-IOV, dados é entregue diretamente para a placa de rede sem passar pela pilha da rede (no sistema operacional, no caso de virtualização). Portanto, não é possível para a equipe NIC de inspecionar ou redirecionar os dados para outro caminho na equipe.  
  
-   **Host nativo qualidade de serviço (QoS)**. Quando QoS políticas são definidas em um nativo ou sistema host e essas políticas invocam limitações de largura de banda mínima, a taxa de transferência geral para uma equipe NIC será menor do que seria sem as políticas de largura de banda no lugar.  
  
-   **Chaminés TCP**. Chaminés TCP não é compatível com NIC agrupamento porque TCP chaminés libera a pilha de rede inteira diretamente à NIC.  
  
-   **802.1 autenticação de x**. 802.1 Autenticação X não deve ser usada com o agrupamento de placa de rede. Algumas opções não permitem a configuração de autenticação 802.1 X e NIC agrupamento na mesma porta.  
  
Para saber sobre como usar o agrupamento de NIC dentro de máquinas virtuais (VMs) que estão em execução em um host do Hyper-V, consulte [NIC agrupamento em máquinas virtuais & #40; VMs & #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md).  
  
## <a name="bkmk_vmq"></a>Agrupamento de NIC e filas de máquina Virtual (VMQs)  
VMQ e agrupamento de NIC funcionam bem juntos; VMQ deve ser habilitado a qualquer momento Hyper-V está habilitado. Dependendo do modo de configuração do switch e o algoritmo de distribuição de carga, a NIC agrupamento ou apresentará funcionalidades VMQ alternar o Hyper-V que mostrem o número de filas disponíveis para ser o menor número de filas compatíveis com qualquer adaptador no grupo (modo de filas Min) ou o número total de filas disponíveis em todos os membros da equipe (modo de soma de filas).  
  
Especificamente, se a equipe é independente de comutador agrupamento modo e a distribuição da carga é definida como modo de porta do Hyper-V ou dinâmico, o número de filas relatados é a soma de todas as filas disponíveis de membros da equipe (modo de soma de filas); Caso contrário, o número de filas relatados é o menor número de filas de suporte por qualquer membro da equipe (modo de filas Min).  
  
Aqui estão as respostas:  
  
-   Quando a equipe independentes do switch está no modo de porta do Hyper-V ou dinâmico o tráfego de entrada para uma porta de comutador do Hyper-V (VM) sempre chegarão no membro da equipe do mesmo. O host pode prever/controle quais membro receberá o tráfego de uma VM específica para que NIC agrupamento pode ser mais cuidadoso sobre quais filas VMQ aloquem em um membro da equipe específica. Agrupamento de NIC, trabalhando com o comutador do Hyper-V, definir o VMQ para uma VM em exatamente um membro da equipe e sabe que o tráfego de entrada alcance fila.  
  
-   Quando a equipe está em qualquer modo dependentes switch (agrupamento estático ou agrupamento de LACP), a opção que a equipe está conectada à controla a distribuição de tráfego de entrada. Software de agrupamento de NIC do host não é possível prever quais equipe membro obterá o tráfego de entrada para uma VM e pode ser que a opção de distribuir o tráfego para uma VM em todos os membros da equipe. Como um resultado do software NIC agrupamento, trabalhando com o comutador do Hyper-V, programas uma fila para a VM em cada membro da equipe, não apenas um membro da equipe do.  
  
-   Quando a equipe está no modo de comutador independente e está usando um algoritmo de distribuição de carga de hash de endereço, o tráfego de entrada sempre serão enviados uma placa de rede (o membro da equipe principal) - todas elas no membro da equipe apenas um. Desde que os outros membros da equipe não estão lidando com o tráfego de entrada com que eles obterem programados a mesma enfileira como membro primário para que se membro primário falhar qualquer outro membro da equipe pode ser usado para pegar o tráfego de entrada e as filas já estão no lugar.  
  
A maioria dos NICs têm filas que podem ser usadas para receber lado dimensionamento RSS () ou VMQ, mas não ambos ao mesmo tempo. Algumas configurações VMQ parecem ser as configurações de filas RSS, mas são realmente configurações nas filas genéricas que use RSS e VMQ dependendo de qual recurso está atualmente em uso. Cada NIC tem, em que ele tem as propriedades avançadas, valores para * RssBaseProcNumber e \*MaxRssProcessors. A seguir estão algumas configurações VMQ que fornecem melhor desempenho do sistema.  
  
-   O ideal é que cada NIC deve ter o * RssBaseProcNumber definido como um número par maior ou igual a 2 (dois). Isso ocorre porque o primeiro processador físico, Core 0 (processadores lógicos 0 e 1), normalmente faz a maioria de processamento do sistema para que o processamento de rede deve ser orientado longe este processador físico. (O algumas arquiteturas de máquina não têm dois processadores lógicos por processador físico para esses computadores o processador base deve ser maior ou igual a 1. Se estiver em dúvida pressuponha que o host está usando um processador lógico 2 por arquitetura de processador físico.)  
  
-   Se a equipe está no modo de soma de filas processadores de membros da equipe devem ser, a limite prático, não sobrepostas. Por exemplo, em um host de 4 núcleos (8 processadores lógicos) com uma equipe de 2 NICs 10 Gbps, você pode definir um dos primeiros para usar um processador base de 2 e usar 4 núcleos; o segundo seria definido para usar processador base 6 e usar 2 núcleos.  
  
-   Se a equipe está no modo de filas Min os conjuntos de processador usados pelos membros da equipe devem ser idênticos.  
  
## <a name="bkmk_hnv"></a>Virtualização de rede NIC agrupamento e Hyper-V (HNV)  
Agrupamento de NIC é totalmente compatível com a virtualização de rede Hyper-V (HNV).  O sistema de gerenciamento de HNV fornece informações para o driver NIC agrupamento que permite o agrupamento de NIC distribuir a carga de uma forma que é otimizada para o tráfego HNV.  
  
## <a name="bkmk_live"></a>Agrupamento de NIC e migração ao vivo  
Agrupamento NIC em VMs não afeta a migração ao vivo. As mesmas regras existem para migração ao vivo ou não NIC agrupamento é configurado na VM.  
  
## <a name="see-also"></a>Consulte também  
[NIC agrupamento em máquinas virtuais & #40; VMs & #41;](../../technologies/nic-teaming/../../technologies/nic-teaming/NIC-Teaming-in-Virtual-Machines--VMs-.md)  
  


