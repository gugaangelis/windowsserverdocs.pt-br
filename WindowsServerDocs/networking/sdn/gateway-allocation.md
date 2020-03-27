---
title: Alocação de largura de banda de gateway
description: ''
manager: dougkim
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: lizross
author: eross-msft
ms.date: 08/22/2018
ms.openlocfilehash: 8e0709360449e1175988b14a963047ee72f26ade
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313032"
---
# <a name="gateway-bandwidth-allocation"></a>Alocação de largura de banda de gateway

>Aplica-se a: Windows Server

No Windows Server 2016, a largura de banda de túnel individual para IPsec, GRE e L3 era uma proporção da capacidade total do gateway. Portanto, os clientes forneceriam a capacidade do gateway com base na largura de banda TCP padrão, esperando isso fora da VM do gateway.

Além disso, a largura de banda máxima do túnel IPsec no gateway era limitada a (3/20)\*capacidade de gateway fornecida pelo cliente. Portanto, por exemplo, se você definir a capacidade do gateway como 100 Mbps, a capacidade do túnel IPsec será de 150 Mbps. As taxas equivalentes para túneis GRE e L3 são 1/5 e 1/2, respectivamente.

Embora isso tenha trabalhado para a maioria das implantações, o modelo de taxa fixa não era apropriado para ambientes de alta taxa de transferência. Mesmo quando as taxas de transferência de dados eram altas (digamos, mais de 40 Gbps), a taxa de transferência máxima de túneis de gateway de SDN é limitada devido a fatores internos.

No Windows Server 2019, para um tipo de túnel, a taxa de transferência máxima é fixa:

-   IPsec = 5 Gbps

-   GRE = 15 Gbps

-   L3 = 5 Gbps

Portanto, mesmo que o host/VM do gateway dê suporte a NICs com uma taxa de transferência muito maior, a taxa de transferência máxima do túnel disponível será corrigida. Outro problema que isso cuida é que o excesso de provisionamento de gateways é arbitrariamente, o que acontece ao fornecer um número muito alto para a capacidade do gateway.

## <a name="gateway-capacity-calculation"></a>Cálculo da capacidade do gateway

O ideal é definir a capacidade de taxa de transferência do gateway para a taxa de transferência disponível para a VM do gateway. Portanto, por exemplo, se você tiver uma única VM de gateway e a taxa de transferência de NIC de host subjacente for de 25 Gbps, a taxa de transferência do gateway também poderá ser definida como 25 Gbps.

Se estiver usando um gateway somente para conexões IPsec, a capacidade fixa máxima disponível será de 5 Gbps. Portanto, por exemplo, se você provisionar conexões IPsec no gateway, só poderá provisionar para uma largura de banda agregada (entrada + saída) como 5 Gbps.

Se estiver usando o gateway para conectividade IPsec e GRE, você poderá provisionar o máximo de 5 Gbps de taxa de transferência IPsec ou um máximo de 15 Gbps de taxa de transferência de GRE. Portanto, por exemplo, se você provisionar 2 Gbps de taxa de transferência IPsec, terá 3 Gbps de taxa de transferência IPsec restante para provisionamento no gateway ou 9 Gbps de taxa de transferência do GRE restante.

Para colocar isso em termos mais matemáticos:

- Capacidade total do gateway = 25 Gbps

- Capacidade IPsec total disponível = 5 Gbps (fixo)

- Capacidade do GRE total disponível = 15 Gbps (fixo)

- Taxa de produtividade IPsec para este gateway = 25/5 = 5 Gbps

- Taxa de taxa de transferência de GRE para este gateway = 25/15 = 5/3 Gbps

Por exemplo, se você alocar 2 Gbps de taxa de transferência IPsec para um cliente:

Capacidade disponível restante no gateway = total de capacidade do gateway – taxa de taxa de transferência IPsec * taxa de transferência IPsec alocada (capacidade usada)

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;25 – 5 * 2 = 15 Gbps

Taxa de transferência IPsec restante que você pode alocar no gateway 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;5-2 = 3 Gbps

Taxa de transferência de GRE restante que você pode alocar no gateway = capacidade restante da taxa de taxa de transferência de gateway/GRE 

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;15 * 3/5 = 9 Gbps

A taxa de taxa de transferência varia dependendo da capacidade total do gateway. Uma coisa a ser observada é que você deve definir a capacidade total para a largura de banda TCP disponível para a VM do gateway. Se você tiver várias VMs hospedadas no gateway, deverá ajustar a capacidade total do gateway adequadamente.

Além disso, se a capacidade do gateway for menor que a capacidade total disponível do túnel, a capacidade total disponível do túnel será definida como a capacidade do gateway. Por exemplo, se você definir a capacidade do gateway para 4 Gbps, a capacidade total disponível para IPsec, L3 e GRE será definida como 4 Gbps, deixando a taxa de taxa de transferência para cada túnel para 1 Gbps.

## <a name="windows-server-2016-behavior"></a>Comportamento do Windows Server 2016

O algoritmo de cálculo da capacidade do gateway para o Windows Server 2016 permanece inalterado. No Windows Server 2016, a largura de banda máxima do túnel IPsec era limitada a (3/20)\*capacidade de gateway em um gateway. As taxas equivalentes para túneis GRE e L3 eram 1/5 e 1/2, respectivamente.

Se você estiver atualizando do Windows Server 2016 para o Windows Server 2019:

1.  **Túneis GRE e L3:** A lógica de alocação do Windows Server 2019 entra em vigor quando os nós do controlador de rede são atualizados para o Windows Server 2019

2.  **Túneis IPsec:** A lógica de alocação de gateway do Windows Server 2016 continua a funcionar até que todos os gateways no pool de gateway sejam atualizados para o Windows Server 2019. Para todos os gateways no pool de gateway, você deve definir o serviço de gateway do Azure como **automático**.

>[!NOTE]
>É possível que, após a atualização para o Windows Server 2019, um gateway se torne excessivamente provisionado (uma vez que a lógica de alocação muda do Windows Server 2016 para o Windows Server 2019). Nesse caso, as conexões existentes no gateway continuam a existir. O recurso REST para o gateway gera um aviso de que o gateway está excessivamente provisionado. Nesse caso, você deve mover algumas conexões para outro gateway.

---