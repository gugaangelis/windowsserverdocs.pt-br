---
title: Desempenho do subsistema de rede ajuste
description: Este tópico faz parte do guia ajuste de desempenho do subsistema de rede para Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c7ef50335a6dcc7dc5187cc30ff1b2dc2c5cdfed
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="network-subsystem-performance-tuning"></a>Desempenho do subsistema de rede ajuste

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

Você pode usar este tópico para obter uma visão geral do subsistema de rede e links para outros tópicos neste guia.

>[!NOTE]
>Além de neste tópico, as seguintes seções deste guia fornecem recomendações de ajuste de desempenho para dispositivos de rede e a pilha de rede.
> - [Escolhendo um adaptador de rede](net-sub-choose-nic.md)
> - [Configurar a ordem das Interfaces de rede](net-sub-interface-metric.md)
> - [Adaptadores de rede de ajuste de desempenho](net-sub-performance-tuning-nics.md)
> - [Contadores de desempenho relacionados à rede](net-sub-performance-counters.md)
> - [Ferramentas de desempenho para cargas de trabalho de rede](net-sub-performance-tools.md)

O subsistema de rede, especialmente para uso intensivo cargas de trabalho de rede, de ajuste de desempenho pode envolver cada camada da arquitetura de rede, que também é chamada a pilha de rede. Essas camadas amplamente são divididas em seções a seguir.

1. **Interface de rede**. Isso é a camada mais baixa na pilha de rede e contém o driver de rede que se comunica diretamente com o adaptador de rede.

2. **Especificação de Interface de Driver (NDIS) de rede**. NDIS expõe interfaces para o driver abaixo dela e as camadas acima dela, como a pilha de protocolo.
  
3. **Pilha de protocolo**. A pilha de protocolo implementa protocolos, como TCP/IP e UDP/IP. Essas camadas expõem a interface de camada de transporte para camadas acima-los.
  
4. **Drivers de sistema**. Normalmente, esses são os clientes que usam uma extensão de dados de transporte (TDX) ou a interface WSK (Winsock Kernel) para expor interfaces para aplicativos de modo de usuário. A interface WSK foi introduzida no Windows Server 2008 e Windows&reg; Vista e ele é exposto pela AFD.sys. A interface melhora o desempenho, eliminando a alternância entre o modo de usuário e modo kernel.
  
5. **Aplicativos de modo de usuário**. Esses são normalmente soluções Microsoft ou aplicativos personalizados.

A tabela a seguir fornece uma ilustração vertical das camadas na pilha da rede, incluindo exemplos de itens que são executados em cada camada.  

![Camadas de pilha de rede](../../media/Network-Subsystem/network-layers.jpg)

