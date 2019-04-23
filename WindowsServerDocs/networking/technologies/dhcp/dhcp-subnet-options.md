---
title: Opções de seleção de sub-rede do DHCP
description: Este tópico fornece informações sobre as opções de seleção de sub-rede do DHCP para configuração de protocolo DHCP (Dynamic Host) no Windows Server 2016.
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-dhcp
ms.topic: get-started-article
ms.assetid: ca19e7d1-e445-48fc-8cf5-e4c45f561607
ms.author: pashort
author: shortpatti
ms.date: 08/17/2018
ms.openlocfilehash: 034ca48ef13a6bdac63ca99ac753fc9826460922
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870807"
---
# <a name="dhcp-subnet-selection-options"></a>Opções de seleção de sub-rede do DHCP

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter informações sobre novas opções de seleção de sub-rede DHCP.

DHCP agora dá suporte à opção 82 \(opção inferior a 5\). Você pode usar essas opções para permitir que os clientes de proxy DHCP e agentes de retransmissão solicitar um endereço IP para uma sub-rede específica e de um intervalo de endereços IP específico e um escopo.  Para obter mais detalhes, consulte **opção 82 Sub opção 5**: [Seleção de Link do RFC 3527 subopção para a opção de informações do agente de retransmissão para DHCPv4](https://tools.ietf.org/html/rfc3527).

Se você estiver usando um agente de retransmissão DHCP que é configurado com a opção DHCP 82, inferiores a opção 5, o agente de retransmissão pode solicitar uma concessão de endereço IP para clientes DHCP de um intervalo de endereço IP específico.


## <a name="option-82-sub-option-5-link-selection-sub-option"></a>Opção 82 Sub opção 5: Opção de Sub de seleção do link

A opção de subdiretório de seleção do Link de agente de retransmissão permite que um agente de retransmissão DHCP especificar uma sub-rede IP do qual o servidor DHCP deve atribuir endereços IP e opções.

Normalmente, os agentes de retransmissão DHCP contam com o endereço IP do Gateway \(GIADDR\) campo para se comunicar com servidores DHCP. No entanto, GIADDR é limitado por suas funções operacionais dois:

1. Para informar o servidor DHCP sobre a sub-rede na qual reside o cliente DHCP que está solicitando a concessão de endereço IP.
2. Para informar o servidor DHCP do endereço IP para usar para se comunicar com o agente de retransmissão.

Em alguns casos, o endereço IP que o agente de retransmissão usa para se comunicar com o servidor DHCP pode ser diferente do intervalo de endereços IP do qual o endereço IP do cliente DHCP deve ser alocado. 

A opção de Link Sub de seleção da opção 82 é útil nessa situação, permitindo que o agente de retransmissão declarar explicitamente a sub-rede da qual ele deseja que o endereço IP alocado na forma de opção do DHCP v4 subopção 82 5.

> [!NOTE]
>
> Todos os endereços IP de agente retransmissão (GIADDR) devem ser parte de um intervalo de endereços IP do escopo do DHCP ativo. Qualquer GIADDR fora os intervalos de endereços IP de escopo do DHCP é considerado uma retransmissão rogue e servidor de DHCP do Windows não a reconhecerá solicitações do cliente desses agentes de retransmissão DHCP.
>
> Um escopo especial pode ser criado para "autorizar" agentes de retransmissão. Criar um escopo com o GIADDR (ou vários se o GIADDR sequenciais endereços IP), excluir o (s) GIADDR da distribuição e, em seguida, ativar o escopo. Isso autorizará os agentes de retransmissão enquanto impede que os endereços GIADDR que está sendo atribuído.


### <a name="use-case-scenario"></a>Cenário de caso de uso

Nesse cenário, uma rede da organização inclui um servidor DHCP e um ponto de acesso sem fio \(AP\) para os usuários convidados. Endereços IP do cliente de convidados são atribuídos do servidor DHCP organização – no entanto, devido a restrições de política de firewall, o servidor DHCP não é possível acessar a rede sem fio de convidados ou clientes sem fio com broadcase mensagens.

Para resolver essa restrição, o ponto de acesso está configurado com a opção 5 do Link seleção Sub para especificar a sub-rede da qual ele deseja que o endereço IP alocado para clientes de convidado, enquanto no GIADDR também especificando o endereço IP da interface interna que leva à rede corporativa.
