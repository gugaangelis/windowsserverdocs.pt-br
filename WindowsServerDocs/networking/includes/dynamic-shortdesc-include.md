---
author: eross-msft
ms.author: lizross
ms.date: 10/02/2018
ms.prod: windows-server
ms:topic: include
ms.openlocfilehash: f2065acb89af4bed4dc525453bb5a294a4e2c3ef
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316491"
---
Com cargas de saída dinâmicas, são distribuídas com base em um hash de portas TCP e endereços IP. O modo dinâmico também reequilibra cargas em tempo real para que um determinado fluxo de saída possa se mover entre os membros da equipe. As cargas de entrada, por outro lado, são distribuídas da mesma maneira que a porta do Hyper-V. Resumindo, o modo dinâmico utiliza os melhores aspectos do hash de endereço e da porta Hyper-V e é o modo de balanceamento de carga de desempenho mais alto. 

