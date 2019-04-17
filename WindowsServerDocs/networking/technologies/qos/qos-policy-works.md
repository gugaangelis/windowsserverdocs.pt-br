---
title: Como funciona a diretiva de QoS
description: Este tópico fornece uma visão geral da política de qualidade de serviço (QoS), que permite que você use a política de grupo para priorizar a largura de banda de tráfego de rede de aplicativos específicos e serviços no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1073308b5939e648fdcc2006acdce76ecf0331c9
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="how-qos-policy-works"></a>Como funciona a diretiva de QoS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Quando iniciado ou obtenção de usuário atualizada ou configurações de política de grupo de configuração do computador para QoS, ocorre o seguinte processo.

1. O mecanismo de política de grupo recupera as configurações de política de grupo de configuração do usuário ou computador do Active Directory.

2. O mecanismo de política de grupo informa a extensão do lado do cliente de QoS que houve alterações nas políticas de QoS.

3. A extensão do lado do cliente de QoS envia uma notificação de evento de política de QoS para o módulo de inspeção de QoS.

4. O módulo de inspeção de QoS recupera as políticas de QoS do usuário ou computador e os armazena.

Quando um novo ponto de extremidade de camada de transporte \ (conexão TCP ou UDP traffic\) é criado, ocorre o seguinte processo.

1. O componente de camada de transporte da pilha TCP/IP informa o módulo de inspeção de QoS.

2. O módulo de inspeção de QoS compara os parâmetros do ponto de extremidade camada de transporte para as políticas de QoS armazenadas.

3. Se uma correspondência for encontrada, o módulo de inspeção de QoS contatos Pacer.sys para criar um fluxo, uma estrutura de dados que contém o valor DSCP e o tráfego de configurações da diretiva de QoS correspondente de otimização. Se houver várias políticas de QoS que correspondem aos parâmetros do ponto de extremidade camada de transporte, a política de QoS mais específica é usada.

4. Pacer.sys armazena o fluxo e retorna um número de fluxo correspondente ao fluxo para o módulo de inspeção de QoS.

5. O módulo de inspeção de QoS retorna o número de fluxo para a camada de transporte.

6. A camada de transporte armazena o número de fluxo com o ponto de extremidade de camada de transporte.

Quando um pacote que corresponde a um ponto de extremidade de camada de transporte marcado com um número de fluxo é enviado, ocorre o seguinte processo.

1. A camada de transporte marca internamente o pacote com o número de fluxo.

2. A camada de rede consulta Pacer.sys para o valor DSCP correspondente para o número de fluxo do pacote.

3. Pacer.sys retorna o valor DSCP para a camada de rede.

4. A camada de rede muda o campo de instruções de IPv4 ou IPv6 tráfego classe ao valor DSCP especificado por Pacer.sys e, para pacotes IPv4, calcula a soma de verificação de cabeçalho IPv4 final.

5. A camada de rede prático, o pacote para a camada de enquadramento.

6. Porque o pacote tiver sido marcado com um número de fluxo, a camada de enquadramento prático, o pacote para Pacer.sys por meio de NDIS 6. x.

7. Pacer.sys usa o número de fluxo do pacote para determinar se o pacote precisa ser limitadas e, em caso afirmativo, agenda o envio do pacote.

8. Pacer.sys entrega o pacote imediatamente \ (se não houver nenhum throttling\ tráfego) ou conforme agendado \ (se houver tráfego throttling\) para NDIS 6. x transmita usando o adaptador de rede apropriado.

Esses processos de QoS baseada em política de fornecem as seguintes vantagens.

- A inspeção de tráfego para determinar se uma política de QoS se aplica é feita o ponto de extremidade do-Transport Layer, em vez de por pacote.

- Não há nenhum impacto no desempenho para o tráfego que não correspondem a uma política de QoS.

- Aplicativos não precisam ser modificados para tirar proveito do serviço diferenciado com base em DSCP ou otimização do tráfego.

- Diretivas de QoS podem ser aplicadas ao tráfego protegido com IPsec.

Para o próximo tópico neste guia, consulte [arquitetura da política de QoS](qos-policy-architecture.md).

Para o primeiro tópico neste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).
