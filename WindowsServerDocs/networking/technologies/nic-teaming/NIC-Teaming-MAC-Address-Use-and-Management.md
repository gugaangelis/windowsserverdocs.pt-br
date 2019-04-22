---
title: Gerenciamento e uso do endereço MAC de agrupamento NIC
description: Quando você configura uma equipe NIC com a opção de modo independente e hash de endereço ou distribuição de carga dinâmico, a equipe usa que o acesso à mídia (MAC) de endereço do membro da equipe de NIC primário no tráfego de saída de controle. O membro da equipe de NIC primário é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.
manager: dougkim
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-nict
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 074a55d2f0a5423af5892ed73dc8a2b8dbda9d5b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824247"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Gerenciamento e uso do endereço MAC de agrupamento NIC

>Aplica-se a: Windows Server 2016

Quando você configura uma equipe NIC com a opção de modo independente e hash de endereço ou distribuição de carga dinâmico, a equipe usa que o acesso à mídia (MAC) de endereço do membro da equipe de NIC primário no tráfego de saída de controle. O membro da equipe de NIC primário é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.  É o primeiro membro da equipe para associar à equipe após criá-lo ou depois que o computador host for reiniciado. Porque o membro da equipe principal pode ser alterado de forma não determinística em cada inicialização, NIC habilitar/desabilitar ação ou outras atividades de reconfiguração, o membro da equipe primária podem alterar e o endereço MAC da equipe podem variar.  
  
Na maioria das situações, isso não causa problemas, mas há alguns casos em que os problemas podem surgir.  
  
Se o membro da equipe principal é removido da equipe e, em seguida, colocado em operação pode haver um conflito de endereço MAC. Para resolver esse conflito, desabilite e, em seguida, habilite a interface de equipe. O processo de desativação e, em seguida, habilitando a interface de equipe faz com que a interface selecionar um novo endereço de MAC os restante dos membros da equipe, eliminando o conflito de endereço MAC.  
  
Você pode definir o endereço MAC do agrupamento NIC para um endereço MAC específico definindo-o na interface primária de equipe, assim como você pode fazer ao configurar o endereço MAC de qualquer placa de rede física.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Usar o endereço MAC em pacotes transmitidos  
Quando você configura um agrupamento NIC no comutador de modo independente e hash de endereço ou distribuição de carga dinâmico, os pacotes de uma única fonte (como uma única VM) simultaneamente é distribuído entre vários membros da equipe. Para impedir que os comutadores confuso e evitar oscilando alarmes de MAC, o endereço MAC de origem é substituído por um endereço MAC diferente dos quadros transmitidos em membros da equipe que não seja membro da equipe principal. Por isso, cada membro da equipe usa um endereço MAC diferente e conflitos de endereços MAC são impedidos até que a falha ocorre.  
  
Quando uma falha é detectada na NIC primária, o software de agrupamento NIC é iniciado usando o endereço de MAC do membro da equipe principal no membro da equipe é escolhido para servir como um membro da equipe de primário temporário (ou seja, aquele que será exibido ao comutador como a equipe principal me mero).  Essa alteração se aplica apenas ao tráfego que estava acontecendo seja enviado o membro da equipe principal com o endereço de MAC do membro da equipe principal como seu endereço MAC de origem. Outro tráfego continua a ser enviada com qualquer fonte da qual endereço de MAC, ele poderia ter usado antes da falha.  
  
Seguem listas que descrevem o comportamento de substituição de endereço MAC de agrupamento NIC, com base em como a equipe é configurada:  
  
1.  **No modo independente de comutador com a distribuição de Hash de endereço**  
  
    -   Todos os pacotes de ARP e NS são enviados em um membro da equipe principal  
  
    -   Todo o tráfego enviado em NICs que não seja membro da equipe principal são enviados com o endereço MAC de origem modificado para corresponder à NIC na qual eles são enviados  
  
    -   Todo o tráfego enviado em um membro da equipe principal é enviado com a fonte original endereço MAC (que pode ser o endereço MAC de origem da equipe)  
  
2.  **No modo independente de comutador com a distribuição de porta Hyper-V**  
  
    -   Cada porta vmSwitch é agrupado com um membro da equipe  
  
    -   Cada pacote é enviado em um membro da equipe ao qual a porta é agrupado com  
  
    -   Nenhuma fonte de substituição de MAC é feita  
  
3.  **No modo independente de comutador com a distribuição dinâmica**  
  
    -   Cada porta vmSwitch é agrupado com um membro da equipe  
  
    -   Todos os pacotes de ARP/NS são enviados em um membro da equipe ao qual a porta é agrupado com  
  
    -   Pacotes enviados em um membro da equipe que é membro da equipe de afinidade não tem nenhuma fonte de substituição de endereço MAC feita  
  
    -   Pacotes enviados em um membro da equipe que não seja membro da equipe afinidade terá a substituição de endereço MAC de origem concluída  
  
4.  **No modo dependentes de Switch (todas as distribuições)**  
  
    -   Nenhuma substituição de endereço MAC de origem é executada  
  
## <a name="related-topics"></a>Tópicos relacionados
- [Agrupamento NIC](NIC-Teaming.md): Neste tópico, nós fornecemos a você uma visão geral do agrupamento de placa de Interface de rede (NIC) no Windows Server 2016. Agrupamento NIC permite que você agrupe entre um e 32 adaptadores de rede Ethernet física em um ou mais adaptadores de rede virtual baseada em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.  

- [Configurações de agrupamento NIC](nic-teaming-settings.md): Neste tópico, podemos lhe dar uma visão geral das propriedades de agrupamento NIC, como agrupamento e modos de balanceamento de carga. Nós também fornecemos a você detalhes sobre a configuração do adaptador em espera e a propriedade de interface de equipe principal. Se você tiver pelo menos dois adaptadores de rede em um agrupamento NIC, você não precisa designar um adaptador de modo de espera para tolerância a falhas.
  
- [Criar um novo agrupamento NIC em uma VM ou computador host](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): Neste tópico, você cria um novo agrupamento NIC em um computador host ou em uma máquina virtual do Hyper-V (VM) executando o Windows Server 2016.

- [Solução de problemas de agrupamento NIC](Troubleshooting-NIC-Teaming.md): Neste tópico, discutiremos maneiras de solucionar problemas de agrupamento NIC, como hardware, firmas de comutador físico e desabilitando ou habilitando adaptadores de rede usando o Windows PowerShell. 
  


