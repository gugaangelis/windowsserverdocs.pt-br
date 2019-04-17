---
title: Agrupamento de gerenciamento e o uso de endereço MAC do NIC
description: Este tópico fornece informações sobre como o agrupamento de NIC usa que o acesso à mídia controla o endereço (MAC) no Windows Server 2016.
manager: brianlic
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
ms.openlocfilehash: 6ab624665c964b100e6b1ed633120299b55e37df
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="nic-teaming-mac-address-use-and-management"></a>Agrupamento de gerenciamento e o uso de endereço MAC do NIC

>Aplica-se a: Windows Server 2016

Quando você configura uma equipe de NIC com Alternar modo independente e hash de endereço ou distribuição de carregamento dinâmico, os usos de equipe que o acesso à mídia (MAC) endereço do membro da equipe de NIC principal no tráfego de saída de controle. O membro da equipe de NIC principal é um adaptador de rede selecionado pelo sistema operacional do conjunto inicial de membros da equipe.  
  
O membro da equipe principal é o primeiro membro da equipe para associar a equipe depois de criá-lo ou depois que o computador host é reiniciado. Como o membro da equipe principal pode mudar de maneira determinística em cada inicialização, NIC habilitar/desabilitar ação ou outras atividades de reconfiguração, o membro da equipe principal podem mudança e o endereço MAC da equipe pode variar.  
  
Na maioria das situações, isso não cause problemas, mas há alguns casos em que problemas podem surgir.  
  
Se o membro da equipe principal é removido da equipe do e colocado em operação pode haver um conflito de endereço MAC. Para resolver o conflito, desabilite e habilite a interface de equipe. O processo de desabilitação e, em seguida, permitindo que a interface de equipe faz com que a interface selecionar um novo endereço MAC de membros da equipe restantes, eliminando o conflito de endereço MAC.  
  
Você pode definir o endereço MAC da equipe da placa de rede para um determinado endereço MAC definindo-o na interface da equipe principal, assim como você pode fazer ao configurar o endereço MAC do NIC qualquer. físico  
  
## <a name="mac-address-use-on-transmitted-packets"></a>Usar o endereço MAC em pacotes transmitidos  
Quando você configura uma equipe de NIC no modo independente do comutador e hash de endereço ou distribuição de carregamento dinâmico, os pacotes de uma única origem (como uma única VM) simultaneamente estão distribuídos em vários membros da equipe. Para impedir que as opções confuso e impedir que o MAC oscilando alarmes, a origem de endereço MAC é substituída por um endereço MAC diferente dos quadros transmitida em membros da equipe que não seja o membro da equipe principal. Por essa razão, cada membro da equipe usa um endereço MAC diferente e conflitos de endereço MAC são impedidos, a menos e até que a falha ocorre.  
  
Quando uma falha é detectada no NIC principal, o software de agrupamento de NIC começa a usar o endereço MAC do membro da equipe principal no membro da equipe que é escolhido para servir como o membro da equipe principal temporários (isto é, aquele que agora será exibido para a opção como o membro da equipe principal).  Essa alteração se aplica apenas ao tráfego que estava acontecendo seja enviado o membro da equipe principal com o endereço MAC do membro da equipe principal como sua origem de endereço MAC. Outro tráfego continua a ser enviado com qualquer endereço MAC que faria usou antes da falha de origem.  
  
A seguir estão listas que descrevem o comportamento de substituição de endereço MAC do NIC agrupamento, com base em como a equipe está configurada:  
  
1.  **No modo Switch independente com a distribuição de Hash de endereço**  
  
    -   Todos os pacotes ARP e NS são enviados no membro da equipe principal  
  
    -   Todo o tráfego enviado em NICs que não seja o membro da equipe principal são enviadas com o endereço MAC de origem modificado para corresponder a placa de rede em que elas são enviadas  
  
    -   Todo o tráfego enviado no membro da equipe principal é enviado com a fonte original endereço MAC (que pode ser o endereço MAC de origem da equipe)  
  
2.  **No modo Switch independente com a distribuição de porta do Hyper-V**  
  
    -   Todas as portas vmSwitch é afinidade para um membro da equipe  
  
    -   Cada pacote é enviado no membro da equipe para o qual a porta é afinidade  
  
    -   Nenhuma fonte de substituição do MAC é feito  
  
3.  **No modo Switch independente com a distribuição dinâmico**  
  
    -   Todas as portas vmSwitch é afinidade para um membro da equipe  
  
    -   Todos os pacotes de ARP/NS são enviados no membro da equipe para o qual a porta é afinidade  
  
    -   Pacotes enviados no que é o membro da equipe seja membro da equipe não têm nenhuma fonte de substituição de endereço MAC concluída  
  
    -   Pacotes enviados em um membro da equipe que não seja o membro da equipe seja terá substituição de endereço MAC de origem concluída  
  
4.  **No modo Alternar dependentes (todas as distribuições)**  
  
    -   Nenhuma fonte de substituição de endereço MAC é realizada  
  
## <a name="see-also"></a>Consulte também  
[Criar uma nova equipe de placa de rede em um computador Host ou VM](Create-a-New-NIC-Team-on-a-Host-Computer-or-VM.md)  
[Agrupamento de NIC](NIC-Teaming.md)  
  


