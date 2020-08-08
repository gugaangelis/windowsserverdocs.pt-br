---
title: Como funciona a política de QoS
description: Este tópico fornece uma visão geral da política de QoS (qualidade de serviço), que permite que você use Política de Grupo para priorizar a largura de banda de tráfego de rede de aplicativos e serviços específicos no Windows Server 2016.
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: fe91bba99000be307ed011cb5636dc49d65c389a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87942526"
---
# <a name="how-qos-policy-works"></a>Como funciona a política de QoS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Ao iniciar ou obter a configuração atualizada do usuário ou do computador Política de Grupo configurações de QoS, ocorre o seguinte processo.

1. O mecanismo de Política de Grupo recupera as configurações de Política de Grupo do usuário ou do computador de Active Directory.

2. O mecanismo de Política de Grupo informa a extensão do lado do cliente de QoS de que houve alterações nas políticas de QoS.

3. A extensão do lado do cliente de QoS envia uma notificação de evento de política de QoS para o módulo de inspeção de QoS.

4. O módulo inspeção de QoS recupera as políticas de QoS de usuário ou computador e as armazena.

Quando uma nova conexão TCP do ponto de extremidade da camada de transporte \( ou o tráfego UDP \) é criado, ocorre o processo a seguir.

1. O componente da camada de transporte da pilha TCP/IP informa o módulo inspeção de QoS.

2. O módulo inspeção de QoS compara os parâmetros do ponto de extremidade da camada de transporte com as políticas de QoS armazenadas.

3. Se uma correspondência for encontrada, o módulo de inspeção de QoS entrará Pacer.sys para criar um fluxo, uma estrutura de dados que contém o valor DSCP e as configurações de limitação de tráfego da política de QoS correspondente. Se houver várias políticas de QoS que correspondam aos parâmetros do ponto de extremidade da camada de transporte, a política de QoS mais específica será usada.

4. Pacer.sys armazena o fluxo e retorna um número de fluxo correspondente ao fluxo para o módulo de inspeção de QoS.

5. O módulo inspeção de QoS retorna o número de fluxo para a camada de transporte.

6. A camada de transporte armazena o número do fluxo com o ponto de extremidade da camada de transporte.

Quando um pacote correspondente a um ponto de extremidade da camada de transporte marcado com um número de fluxo é enviado, ocorre o processo a seguir.

1. A camada de transporte marca internamente o pacote com o número do fluxo.

2. A camada de rede consulta Pacer.sys para o valor DSCP correspondente ao número de fluxo do pacote.

3. Pacer.sys retorna o valor DSCP para a camada de rede.

4. A camada de rede altera o campo de classe do tráfego IPv4 ou do protocolo IPv6 para o valor DSCP especificado por Pacer.sys e, para pacotes IPv4, calcula a soma de verificação do cabeçalho IPv4 final.

5. A camada de rede entrega o pacote à camada de enquadramento.

6. Como o pacote foi marcado com um número de fluxo, a camada de enquadramento distribui o pacote para Pacer.sys por meio do NDIS 6. x.

7. Pacer.sys usa o número de fluxo do pacote para determinar se o pacote precisa ser limitado e, em caso afirmativo, agenda o pacote para envio.

8. Pacer.sys distribui o pacote imediatamente se não houver \( limitação de tráfego \) ou conforme agendado \( se houver limitação de tráfego \) para o NDIS 6. x para transmissão no adaptador de rede apropriado.

Esses processos de QoS baseado em políticas oferecem as seguintes vantagens.

- A inspeção do tráfego para determinar se uma política de QoS se aplica é feita por ponto de extremidade de camada por transporte, em vez de por pacote.

- Não há impacto no desempenho do tráfego que não corresponde a uma política de QoS.

- Os aplicativos não precisam ser modificados para tirar proveito do serviço diferenciado baseado em DSCP ou da limitação de tráfego.

- As políticas de QoS podem ser aplicadas ao tráfego protegido com IPsec.

Para o próximo tópico deste guia, consulte [arquitetura de política de QoS](qos-policy-architecture.md).

Para o primeiro tópico deste guia, consulte [política de QoS (qualidade de serviço)](qos-policy-top.md).
