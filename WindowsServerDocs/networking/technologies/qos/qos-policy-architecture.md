---
title: Arquitetura de política de QoS
description: Este tópico fornece uma visão geral da política de QoS (qualidade de serviço), que permite que você use Política de Grupo para priorizar a largura de banda de tráfego de rede de aplicativos e serviços específicos no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 25097cb8-b9b1-41c9-b3c7-3610a032e0d8
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 609d3f28465380b7d15648cfeb73070a39b9362f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395976"
---
# <a name="qos-policy-architecture"></a>Arquitetura de política de QoS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Você pode usar este tópico para saber mais sobre a arquitetura da política de QoS.

A figura a seguir mostra a arquitetura da QoS baseada em políticas.

![Arquitetura da política de QoS](../../media/QoS/QoS-Policy-Architecture.jpg)

A arquitetura da QoS baseada em políticas consiste nos seguintes componentes:

- **Política de grupo serviço de cliente**. Um serviço do Windows que gerencia configurações do usuário e do computador Política de Grupo.

- **Política de grupo mecanismo**. Um componente do serviço de cliente do Política de Grupo que recupera as configurações do usuário e do computador Política de Grupo de Active Directory na inicialização e verifica periodicamente se há alterações @no__t padrão, a cada 90 minutos @ no__t-1. Se forem detectadas alterações, o mecanismo de Política de Grupo recuperará as novas configurações de Política de Grupo. O mecanismo de Política de Grupo processa os GPOs de entrada e informa a extensão do lado do cliente QoS quando as políticas de QoS são atualizadas.

- **Extensão do lado do cliente QoS**. Um componente do serviço de cliente do Política de Grupo que aguarda uma indicação do mecanismo de Política de Grupo que as políticas de QoS alteraram e informam o módulo de inspeção de QoS.

- **Pilha de TCP/IP**. A pilha TCP/IP que inclui suporte integrado para IPv4 e IPv6 e dá suporte à plataforma de filtragem do Windows. 

- **Inspeção de QoS**. Módulo um componente na pilha TCP/IP que aguarda indicações de alterações de política de QoS da extensão do lado do cliente QoS, recupera as configurações da política de QoS e interage com a camada de transporte e o Pacer. sys para marcar internamente o tráfego que corresponde à QoS Policie.

- **NDIS 6. x**. Uma interface padrão entre drivers de rede de modo kernel e o sistema operacional em sistemas operacionais Windows Server e cliente. O NDIS 6. x dá suporte a filtros leves, que é um modelo de driver simplificado para drivers intermediários NDIS e drivers de miniporta que fornece melhor desempenho.

- **Interface do provedor de rede QoS \(NPI @ no__t-2**. Uma interface para drivers do modo kernel para interagir com o Pacer. sys.

- **Pacer. sys**. Um driver de filtro simples do NDIS 6. x que controla o agendamento de pacotes para a QoS baseada em políticas e para o tráfego de aplicativos que usam o QoS genérico \(GQoS @ no__t-1 e o controle de tráfego \(TC @ no__t-3 APIs. O Pacer. sys substituiu o Psched. sys no Windows Server 2003 e no Windows XP. O Pacer. sys é instalado com o componente de Agendador de pacotes QoS nas propriedades de uma conexão de rede ou adaptador.

Para o próximo tópico deste guia, consulte [cenários de política de QoS](qos-policy-scenarios.md).

Para o primeiro tópico deste guia, consulte [política de QoS (qualidade de serviço)](qos-policy-top.md).

