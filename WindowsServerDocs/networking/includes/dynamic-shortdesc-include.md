---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms:topic: include
ms.openlocfilehash: f07840220dbbe955a47879b3090ddd340c8c514f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87952810"
---
Com cargas de saída dinâmicas, são distribuídas com base em um hash de portas TCP e endereços IP. O modo dinâmico também reequilibra cargas em tempo real para que um determinado fluxo de saída possa se mover entre os membros da equipe. As cargas de entrada, por outro lado, são distribuídas da mesma maneira que a porta do Hyper-V. Resumindo, o modo dinâmico utiliza os melhores aspectos do hash de endereço e da porta Hyper-V e é o modo de balanceamento de carga de desempenho mais alto.

