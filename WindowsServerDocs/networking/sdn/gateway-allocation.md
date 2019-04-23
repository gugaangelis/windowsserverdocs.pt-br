---
title: Alocação de largura de banda de gateway
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: 3c259b96e1a8ee27888a5cccc50b285a2f7cb8c6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850427"
---
# <a name="gateway-bandwidth-allocation"></a>Alocação de largura de banda de gateway

>Aplica-se a: Windows Server

No Windows Server 2016, a largura de banda de túnel individuais para IPsec, GRE e L3 era uma proporção de capacidade total do gateway. Portanto, os clientes podem fornecer a capacidade do gateway com base na largura de banda TCP padrão esperando isso da VM do gateway.

Além disso, o IPsec túnel largura de banda máxima no gateway estava limitada a (3/20)\*capacidade do Gateway fornecido pelo cliente. Portanto, por exemplo, se você definir a capacidade do gateway para 100 Mbps, em seguida, a capacidade de túnel IPsec seria 150 Mbps. As taxas equivalentes para túneis GRE e L3 são de 1/5 e 1/2, respectivamente.

Embora isso funcionou para a maioria das implantações, o modelo de taxa fixa não era adequado para ambientes de alta taxa de transferência. Até mesmo quando as taxas de transferência de dados foram altas (digamos, maior do que 40 Gbps), a taxa de transferência máxima de túneis de gateway SDN limitado devido a fatores internos.

No Windows Server de 2019, para um tipo de túnel, a taxa de transferência máxima é fixo:

-   IPsec = 5 Gbps

-   GRE = 15 Gbps

-   L3 = 5 Gbps

Assim, mesmo se seu host/VM do gateway dá suporte a NICs com a taxa de transferência mais alta, a taxa de transferência máxima de túnel disponíveis é fixa. Outro problema que ele se encarrega de é arbitrariamente excesso de provisionamento gateways, o que acontece ao fornecer um número muito alto para a capacidade do gateway.

## <a name="gateway-capacity-calculation"></a>Cálculo de capacidade do gateway

O ideal é que você definir a capacidade de taxa de transferência do gateway para a taxa de transferência disponível para o gateway de VM. Portanto, por exemplo, se você tiver um único gateway de VM e a taxa de transferência NIC de host subjacente é 25 Gbps, a taxa de transferência do gateway pode ser definida como 25 Gbps também.

Se usar um gateway apenas para conexões IPsec, a capacidade máxima de correção disponível é 5 Gbps. Portanto, por exemplo, se você provisionar conexões IPsec no gateway, você só poderá provisionar a largura de banda agregada (entrada + saída) como 5 Gbps.

Se usar o gateway para conectividade IPsec e GRE, você pode provisionar a taxa de transferência máxima 5 Gbps de IPsec ou a taxa de transferência máxima 15 Gbps de GRE. Portanto, por exemplo, se você provisionar a taxa de transferência de 2 Gbps do IPsec, você ter 3 Gbps do IPsec taxa de transferência da esquerda para a provisão no gateway ou 9 Gbps de GRE taxa de transferência à esquerda.

Para colocar isso em termos matemáticos mais:

- Total de capacidade do gateway = 25 Gbps

- Total de capacidade disponível do IPsec = 5 Gbps (fixas)

- Total de capacidade disponível do GRE = 15 Gbps (fixas)

- Taxa de taxa de transferência de IPsec para este gateway = 5/25 = 5 Gbps

- Taxa de taxa de transferência GRE para este gateway = 25/15 = 5/3 Gbps

Por exemplo, se você alocar a taxa de transferência de 2 Gbps de IPsec para um cliente:

Capacidade disponível no gateway do restante = Total capacidade do gateway – a taxa de produtividade IPsec * taxa de transferência do IPsec alocado (capacidade utilizada)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25–5*2 = 15 Gbps

Taxa de transferência de IPsec restante que você pode alocar no gateway 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 Gbps

Taxa de transferência GRE de restante que pode ser alocada no gateway = capacidade restante de taxa de taxa de transferência/GRE de gateway 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15*3/5 = 9 Gbps

A proporção de taxa de transferência varia de acordo com a capacidade total do gateway. Uma coisa a observar é que você deve definir a capacidade total para a largura de banda TCP disponível para a VM do gateway. Se você tiver várias VMs hospedadas no gateway, você deve ajustar a capacidade total do gateway de acordo.

Além disso, se a capacidade do gateway for menor que a capacidade total de túnel disponíveis, a capacidade de túnel disponível total é definida para a capacidade do gateway. Por exemplo, se você definir a capacidade do gateway como 4 Gbps, a capacidade total disponível para o GRE, IPsec e L3 é definida como 4 Gbps, deixando a proporção de taxa de transferência para cada túnel para 1 Gbps.

## <a name="windows-server-2016-behavior"></a>Comportamento do Windows Server 2016

O algoritmo de cálculo de capacidade do gateway do Windows Server 2016 permanece inalterado. No Windows Server 2016, a largura de banda de túnel IPsec máximo era limitada a (3/20)\*capacidade do gateway em um gateway. As taxas equivalentes para túneis GRE e L3 foram 1/5 e 1/2, respectivamente.

Se você estiver atualizando do Windows Server 2016 para Windows Server 2019:

1.  **Túneis GRE e L3:** A lógica de alocação do Windows Server 2019 entra em vigor depois que nós de controlador de rede são atualizados para Windows Server 2019

2.  **Túneis IPSec:** A lógica de alocação de gateway do Windows Server 2016 continua a funcionar até que todos os gateways no pool de gateway atualizados para Windows Server 2019. Para todos os gateways no pool de gateway, você deve definir o serviço de gateway do Azure **automática**.

>[!NOTE]
>É possível que após a atualização para Windows Server 2019, um gateway se torna provisionado em excesso (como a lógica de alocação muda de Windows Server 2016 para Windows Server 2019). Nesse caso, as conexões existentes no gateway continuam existindo. O recurso REST para o Gateway gera um aviso de que o gateway é provisionado em excesso. Nesse caso, você deve mover algumas conexões para outro gateway.

---