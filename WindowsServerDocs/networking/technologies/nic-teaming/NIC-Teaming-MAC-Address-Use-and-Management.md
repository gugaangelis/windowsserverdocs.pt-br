---
title: Gerenciamento e uso do endereço MAC de agrupamento NIC
description: Quando você configura uma equipe NIC com o modo independente de comutador e endereço hash ou distribuição dinâmica de carga, a equipe usa o endereço MAC (controle de acesso à mídia) do membro da equipe NIC primário no tráfego de saída. O membro da equipe NIC primário é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.
manager: dougkim
ms.prod: windows-server
ms.technology: networking-nict
ms.topic: article
ms.assetid: 26d105e0-afc3-44b5-bb5e-0c884a4c5d62
ms.author: lizross
author: eross-msft
ms.openlocfilehash: a1671b16bdadfcd159bc728f2d39ec45ad82fc0f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854719"
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Gerenciamento e uso do endereço MAC de agrupamento NIC

>Aplica-se ao Windows Server 2016

Quando você configura uma equipe NIC com o modo independente de comutador e endereço hash ou distribuição dinâmica de carga, a equipe usa o endereço MAC (controle de acesso à mídia) do membro da equipe NIC primário no tráfego de saída. O membro da equipe NIC primário é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.  É o primeiro membro da equipe a ser associado à equipe depois que você criá-la ou após a reinicialização do computador host. Como o membro da equipe principal pode ser alterado de forma não determinística em cada inicialização, a ação desabilitar/habilitar NIC ou outras atividades de reconfiguração, o membro da equipe principal pode mudar e o endereço MAC da equipe pode variar.  
  
Na maioria das situações, isso não causa problemas, mas há alguns casos em que podem surgir problemas.  
  
Se o membro da equipe principal for removido da equipe e colocado em operação, pode haver um conflito de endereço MAC. Para resolver esse conflito, desabilite e habilite a interface de equipe. O processo de desabilitar e habilitar a interface de equipe faz com que a interface selecione um novo endereço MAC dos membros da equipe restantes, eliminando assim o conflito de endereços MAC.  
  
Você pode definir o endereço MAC da equipe NIC para um endereço MAC específico definindo-o na interface da equipe principal, assim como você pode fazer ao configurar o endereço MAC de qualquer NIC física.  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Uso de endereço MAC em pacotes transmitidos  
Quando você configura uma equipe NIC no modo independente de switch e de endereço de hash ou de distribuição de carga dinâmica, os pacotes de uma única fonte (como uma única VM) são distribuídos simultaneamente entre vários membros da equipe. Para evitar que os comutadores fiquem confusos e impeçam alarmes de oscilação de MAC, o endereço MAC de origem é substituído por um endereço MAC diferente nos quadros transmitidos em membros da equipe que não sejam o membro da equipe principal. Por isso, cada membro da equipe usa um endereço MAC diferente e os conflitos de endereço MAC são evitados, a menos que e até que ocorra uma falha.  
  
Quando uma falha é detectada na NIC primária, o software de agrupamento NIC começa a usar o endereço MAC do membro da equipe principal no membro da equipe que é escolhido para servir como o membro da equipe primária temporária (ou seja, aquele que agora será exibido para a opção como a equipe principal membro).  Essa alteração se aplica somente ao tráfego que será enviado no membro da equipe principal com o endereço MAC do membro da equipe principal como seu endereço MAC de origem. Outro tráfego continua a ser enviado com qualquer endereço MAC de origem que ele teria usado antes da falha.  
  
Veja a seguir as listas que descrevem o comportamento de substituição de endereço MAC de agrupamento NIC, com base em como a equipe está configurada:  
  
1.  **Em modo independente de opção com distribuição de hash de endereço**  
  
    -   Todos os pacotes ARP e NS são enviados no membro da equipe principal  
  
    -   Todo o tráfego enviado em NICs diferentes do membro da equipe principal é enviado com o endereço MAC de origem modificado para corresponder à NIC na qual eles são enviados  
  
    -   Todo o tráfego enviado no membro da equipe principal é enviado com o endereço MAC de origem original (que pode ser o endereço MAC de origem da equipe)  
  
2.  **Em Alternar modo independente com a distribuição de porta do Hyper-V**  
  
    -   Cada porta vmSwitch é relacionados a um membro da equipe  
  
    -   Cada pacote é enviado no membro da equipe para o qual a porta está relacionados  
  
    -   Nenhuma substituição de MAC de origem foi feita  
  
3.  **Em modo independente de opção com distribuição dinâmica**  
  
    -   Cada porta vmSwitch é relacionados a um membro da equipe  
  
    -   Todos os pacotes ARP/NS são enviados no membro da equipe para o qual a porta está relacionados  
  
    -   Os pacotes enviados no membro da equipe que é o membro da equipe do relacionados não têm nenhuma substituição de endereço MAC de origem concluída  
  
    -   Os pacotes enviados em um membro da equipe que não seja o membro da equipe do relacionados terão a substituição do endereço MAC de origem concluída  
  
4.  **No modo dependente de switch (todas as distribuições)**  
  
    -   Nenhuma substituição de endereço MAC de origem é executada  
  
## <a name="related-topics"></a>Tópicos relacionados
- [Agrupamento NIC](NIC-Teaming.md): neste tópico, fornecemos uma visão geral do agrupamento NIC (placa de interface de rede) no Windows Server 2016. O agrupamento NIC permite que você agrupe entre um e 32 adaptadores de rede Ethernet físicos em um ou mais adaptadores de rede virtual baseados em software. Esses adaptadores de rede virtual oferecem desempenho rápido e tolerância a falhas no caso de uma falha do adaptador de rede.  

- [Configurações de agrupamento NIC](nic-teaming-settings.md): neste tópico, fornecemos uma visão geral das propriedades da equipe NIC, como os modos de agrupamento e balanceamento de carga. Também fornecemos detalhes sobre a configuração do adaptador em espera e a propriedade da interface da equipe principal. Se você tiver pelo menos dois adaptadores de rede em uma equipe NIC, não será necessário designar um adaptador em espera para tolerância a falhas.
  
- [Criar uma nova equipe NIC em um computador host ou VM](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md): neste tópico, você cria uma nova equipe NIC em um computador host ou em uma VM (máquina virtual) do Hyper-V que executa o Windows Server 2016.

- [Solução de problemas de agrupamento NIC](Troubleshooting-NIC-Teaming.md): neste tópico, discutimos maneiras de solucionar problemas de agrupamento de NIC, como hardware, comutador físico de valores e desabilitar ou habilitar adaptadores de rede usando o Windows PowerShell. 
  


