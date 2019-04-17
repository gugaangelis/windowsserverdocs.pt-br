---
title: Arquitetura de diretiva de QoS
description: Este tópico fornece uma visão geral da política de qualidade de serviço (QoS), que permite que você use a política de grupo para priorizar a largura de banda de tráfego de rede de aplicativos específicos e serviços no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 00d36604c57add6bf9f45b0166b08c1fb15be467
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-architecture"></a>Arquitetura de diretiva de QoS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para saber mais sobre a arquitetura da política de QoS.

A figura a seguir mostra a arquitetura de QoS baseada em política.

![Arquitetura de política de QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

A arquitetura de QoS baseada em política consiste nos seguintes componentes:

- **Serviço de cliente de política de grupo**. Um serviço do Windows que gerencia as configurações da política de grupo de usuários e computadores.

- **Mecanismo de política de grupo**. Um componente do serviço de cliente de política de grupo que recupera configurações da política de grupo de usuários e computadores do Active Directory durante a inicialização e periodicamente verifica se há alterações \ (por padrão, cada 90 minutes\). Se alterações forem detectadas, o mecanismo de política de grupo recupera as novas configurações de política de grupo. O mecanismo de política de grupo processa os GPOs de entrada e informa a extensão do lado cliente QoS quando as políticas de QoS são atualizadas.

- **Extensão do lado do cliente de QoS**. Um componente do serviço de cliente de política de grupo que aguarda uma indicação do mecanismo de política de grupo que as políticas de QoS foram alteradas e informa o módulo de inspeção de QoS.

- **Pilha de TCP/IP**. A pilha de TCP/IP que inclui suporte integrado para IPv4 e IPv6 e dá suporte a plataforma para filtros do Windows. 

- **Inspeção de QoS**. Módulo um componente dentro da pilha de TCP/IP que aguarda indicações de alterações de política de QoS da extensão do lado do cliente QoS, recupera as configurações de política de QoS e interage com a camada de transporte e Pacer.sys para marcar internamente o tráfego que corresponde as políticas de QoS.

- **NDIS 6. x**. Uma interface padrão entre drivers de rede de modo kernel e o sistema operacional nos sistemas operacionais Windows Server e o cliente. NDIS 6. x suporta leves filtros, que é um modelo de driver simplificado para drivers NDIS intermediários e drivers de miniporta que proporciona melhor desempenho.

- **Provedor de rede QoS Interface \(NPI\)**. Uma interface para drivers de modo kernel interagir com Pacer.sys.

- **Pacer. sys**. Um driver de filtro simples de 6. x NDIS que controla o agendamento de pacotes para QoS baseada em política de e para o tráfego de aplicativos que usam o controle de tráfego \(TC\) APIs e \(GQoS\) QoS genérico. Pacer.sys substituído Psched.sys no Windows Server 2003 e Windows XP. Pacer.sys é instalado com o componente o Agendador de pacotes nas propriedades de uma conexão de rede ou um adaptador.

Para o próximo tópico neste guia, consulte [cenários de política de QoS](qos-policy-scenarios.md).

Para o primeiro tópico neste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).

