---
title: Arquitetura de política de QoS
description: Este tópico fornece uma visão geral da política de qualidade de serviço (QoS), que permite que você use a diretiva de grupo para priorizar a largura de banda de tráfego de rede de aplicativos e serviços no Windows Server 2016 específicos.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bad37ba3558137b02ae495fe8dd9be2c903cdd97
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843137"
---
# <a name="qos-policy-architecture"></a>Arquitetura de política de QoS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre a arquitetura de política de QoS.

A figura a seguir mostra a arquitetura da QoS baseada em diretivas.

![Arquitetura de política de QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

A arquitetura da QoS baseada em diretivas consiste dos seguintes componentes:

- **Serviço de cliente de diretiva de grupo**. Um serviço do Windows que gerencia as configurações da diretiva de grupo de usuários e computador.

- **Mecanismo de diretiva de grupo**. Um componente do serviço de cliente de diretiva de grupo que recupera configurações da diretiva de grupo de usuários e computadores do Active Directory durante a inicialização e periodicamente verifica se há alterações \(por padrão, a cada 90 minutos\). Se forem detectadas alterações, o mecanismo da diretiva de grupo recupera as novas configurações de diretiva de grupo. O mecanismo da diretiva de grupo processa os GPOs de entrada e informa à extensão de lado do cliente de QoS quando as políticas de QoS são atualizadas.

- **Extensão do lado do cliente de QoS**. Um componente do serviço de cliente de diretiva de grupo que aguarda uma indicação do mecanismo de diretiva de grupo que as políticas de QoS foram alteradas e informa o módulo de inspeção de QoS.

- **TCP/IP Stack**. A pilha de TCP/IP que inclui suporte integrado a IPv4 e IPv6 e dá suporte a Windows Filtering Platform. 

- **Inspeção de QoS**. Componente de um módulo dentro do TCP/IP stack que aguarda indicações de alterações de política de QoS da extensão do lado do cliente de QoS, recupera as configurações de política de QoS e interage com a camada de transporte e o Pacer. sys para marcar internamente o tráfego que coincide com o QoS políticas.

- **NDIS 6.x**. Uma interface padrão entre drivers de rede de modo kernel e o sistema operacional em sistemas operacionais Windows Server e o cliente. NDIS 6.x dá suporte a filtros simples, que é um modelo de driver simplificado para drivers NDIS intermediários e drivers de miniporta que oferece melhor desempenho.

- **Interface de provedor de rede de QoS \(NPI\)**. Uma interface para drivers do modo kernel interajam com o Pacer. sys.

- **O Pacer. sys**. Um driver de filtro simples do NDIS 6. x que controla o agendamento de pacotes de QoS baseada em diretivas e para o tráfego de aplicativos que usam o QoS genérica \(GQoS\) e o controle de tráfego \(TC\) APIs. O Pacer. sys substituído Psched no Windows Server 2003 e Windows XP. O Pacer. sys é instalado com o componente Agendador de pacotes QoS das propriedades de uma conexão de rede ou o adaptador.

Para o próximo tópico neste guia, consulte [cenários de política de QoS](qos-policy-scenarios.md).

Para o primeiro tópico deste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).

