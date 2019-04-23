---
title: Como funciona a diretiva de QoS
description: Este tópico fornece uma visão geral da política de qualidade de serviço (QoS), que permite que você use a diretiva de grupo para priorizar a largura de banda de tráfego de rede de aplicativos e serviços no Windows Server 2016 específicos.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 272272c833bb38924f1daa5561037901f6ff4e25
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59864277"
---
# <a name="how-qos-policy-works"></a>Como funciona a diretiva de QoS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Quando se inicializa ou obtém o usuário atualizada ou configurações de diretiva de grupo de configuração do computador para o QoS, ocorre o processo a seguir.

1. O mecanismo da diretiva de grupo recupera as configurações de diretiva de grupo de configuração de usuário ou computador do Active Directory.

2. O mecanismo da diretiva de grupo informa à extensão do lado do cliente de QoS que houve alterações nas políticas de QoS.

3. A extensão do lado do cliente de QoS envia uma notificação de eventos de política de QoS para o módulo de inspeção de QoS.

4. O módulo de inspeção de QoS recupera as políticas de QoS de usuário ou computador e os armazena.

Quando um novo ponto de extremidade da camada de transporte \(TCP conexão ou tráfego UDP\) é criado, o seguinte processo ocorre.

1. O componente de camada de transporte do TCP/IP stack informa o módulo de inspeção de QoS.

2. O módulo de inspeção de QoS compara os parâmetros do ponto de extremidade de camada de transporte para as diretivas de QoS armazenadas.

3. Se uma correspondência for encontrada, o módulo de inspeção de QoS entra em contato com o Pacer. sys para criar um fluxo, uma estrutura de dados que contém o valor DSCP e o tráfego de configurações da política de QoS de correspondência de limitação. Se houver várias diretivas de QoS que correspondam aos parâmetros do ponto de extremidade de camada de transporte, a política de QoS mais específica é usada.

4. O Pacer. sys armazena o fluxo e retorna um número de fluxo correspondente ao fluxo para o módulo de inspeção de QoS.

5. O módulo de inspeção de QoS retorna o número do fluxo para a camada de transporte.

6. A camada de transporte armazena o número do fluxo com o ponto de extremidade da camada de transporte.

Quando um pacote que corresponde a um ponto de extremidade da camada de transporte marcado com um número de fluxo é enviado, ocorre o seguinte processo.

1. A camada de transporte marca internamente o pacote com o número do fluxo.

2. A camada de rede consulta o Pacer. sys para o valor DSCP correspondente ao número de fluxo do pacote.

3. O Pacer. sys retorna o valor DSCP para a camada de rede.

4. A camada de rede altera o campo TOS do IPv4 ou o campo Traffic Class do IPv6 para o valor DSCP especificado por Pacer. sys e, para os pacotes IPv4, calcula a soma de verificação de cabeçalho IPv4 final.

5. A camada de rede entrega o pacote para a camada de delimitação de quadros.

6. Porque o pacote foi marcado com um número de fluxo, a camada de delimitação de quadros o entrega a Pacer. sys por meio da NDIS 6.x.

7. O Pacer. sys usa o número de fluxo do pacote para determinar se o pacote precisa ser limitado e, nesse caso, agenda o envio do pacote.

8. O Pacer. sys entrega o pacote imediatamente \(se não houver nenhum tráfego limitação\) ou conforme agendado \(se não houver tráfego limitação\) para NDIS 6.x para transmissão de adaptador de rede apropriado.

Esses processos de QoS baseado em políticas fornecem as seguintes vantagens.

- A inspeção do tráfego para determinar se uma política de QoS se aplica é feita o ponto de extremidade de camada por transporte, em vez de por pacote.

- Não há nenhum impacto no desempenho para o tráfego que não corresponde a uma política de QoS.

- Aplicativos não precisam ser modificados para tirar proveito do serviço diferenciado com base em DSCP ou otimização do tráfego.

- As políticas de QoS podem aplicar ao tráfego protegido com IPsec.

Para o próximo tópico neste guia, consulte [arquitetura de política de QoS](qos-policy-architecture.md).

Para o primeiro tópico deste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).
