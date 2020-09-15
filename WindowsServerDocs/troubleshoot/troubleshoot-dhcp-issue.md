---
title: Guia de solução de problemas do DHCP (protocolo de configuração dinâmica de hosts)
description: Este Artilce introduz o guia de solução de problemas de DHCP.
manager: dcscontentpm
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 92a7984fe2070ac194aa01a5a9aa63e85b7aa45d
ms.sourcegitcommit: 7cacfc38982c6006bee4eb756bcda353c4d3dd75
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/14/2020
ms.locfileid: "90078613"
---
# <a name="troubleshooting-guide-for-dynamic-host-configuration-protocol-dhcp"></a>Guia de solução de problemas do DHCP (protocolo de configuração dinâmica de hosts)

Para qualquer dispositivo (como um computador ou telefone) ser capaz de operar em uma rede, ele deve ser atribuído a um endereço IP. Você pode atribuir um endereço IP manual ou automaticamente. A atribuição automática é tratada pelo serviço DHCP (Microsoft ou servidor de terceiros).

Neste artigo, discutiremos as etapas gerais de solução de problemas para o cliente e o servidor DHCP do Microsoft IPv4.

## <a name="more-information"></a>Mais informações

O procedimento para atribuição de endereço IPv4 geralmente envolve três componentes principais:

- Um dispositivo cliente DHCP que precisa obter um endereço IP

- Um serviço DHCP que fornece endereços IP para o cliente com base em configurações específicas

- Um auxiliar/IP de agente de retransmissão DHCP para enviar solicitações de difusão DHCP a um segmento de rede diferente

Uma comunicação de cliente para servidor DHCP consiste em três tipos de interação entre os dois pares:

- **Dora com base em difusão** (descoberta, oferta, solicitação, confirmação). O processo é composto pelas seguintes etapas:

    - O cliente DHCP envia uma solicitação de difusão de descoberta de DHCP para todos os servidores DHCP disponíveis no intervalo.

    - Uma resposta de difusão de oferta DHCP é recebida do servidor DHCP, oferecendo uma concessão de endereço IP disponível.

    - A solicitação de difusão de cliente DHCP solicita a concessão de endereço IP oferecido e a confirmação de difusão DHCP no final.

    - Se o cliente DHCP e o servidor estiverem localizados em diferentes segmentos de rede lógica, um agente de retransmissão DHCP atuará em um encaminhador, enviando os pacotes de difusão DHCP para frente e para trás entre os pares.

- **Solicitações de renovação de DHCP unicast**: elas são enviadas diretamente ao servidor DHCP do cliente DHCP para renovar a atribuição de endereço ip após 50% do tempo de concessão do endereço IP.

- **Reassociar solicitações de difusão DHCP**: elas são feitas em qualquer servidor DHCP dentro do intervalo do cliente. Eles são enviados após 87,5% da duração da concessão do endereço IP porque isso indica que a solicitação unicast direcionada não funcionou. Para o processo DORA, esse processo envolve uma comunicação de agente de retransmissão DHCP.

Se um cliente DHCP da Microsoft não receber um endereço DHCP IPv4 válido, provavelmente o cliente será configurado para usar um endereço APIPA. Para obter mais informações, consulte o seguinte artigo da base de dados de conhecimento: [220874](https://support.microsoft.com/help/220874) como usar o endereçamento TCP/IP automático sem um servidor DHCP

Toda a comunicação é feita nas portas UDP 67 e 68. Para obter mais informações, consulte o seguinte artigo da base de dados de conhecimento: Noções básicas de [169289](https://support.microsoft.com/help/169289) DHCP (protocolo de configuração de host dinâmico).