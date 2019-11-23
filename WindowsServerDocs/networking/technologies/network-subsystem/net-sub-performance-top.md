---
title: Ajuste de desempenho do subsistema de rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 02a1abc9de04926740309081397e0bd92fe74b88
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401883"
---
# <a name="network-subsystem-performance-tuning"></a>Ajuste de desempenho do subsistema de rede

>Aplicável a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter uma visão geral do subsistema de rede e links para outros tópicos deste guia.

>[!NOTE]
>Além deste tópico, as seguintes seções deste guia fornecem recomendações de ajuste de desempenho para dispositivos de rede e a pilha de rede.
> - [Escolhendo um adaptador de rede](net-sub-choose-nic.md)
> - [Configurar a ordem das interfaces de rede](net-sub-interface-metric.md)
> - [Adaptadores de rede de ajuste de desempenho](net-sub-performance-tuning-nics.md)
> - [Contadores de desempenho relacionados à rede](net-sub-performance-counters.md)
> - [Ferramentas de desempenho para cargas de trabalho de rede](net-sub-performance-tools.md)

O ajuste de desempenho do subsistema de rede, especialmente para cargas de trabalho com uso intensivo de rede, pode envolver cada camada da arquitetura de rede, que também é chamada de pilha de rede. Essas camadas são amplamente divididas nas seções a seguir.

1. **Interface de rede**. Essa é a camada mais baixa na pilha de rede e contém o driver de rede que se comunica diretamente com o adaptador de rede.

2. **NDIS (Network Driver Interface Specification)** . O NDIS expõe interfaces para o driver abaixo dele e para as camadas acima dela, como a pilha de protocolos.
  
3. **Pilha de protocolos**. A pilha de protocolos implementa protocolos como TCP/IP e UDP/IP. Essas camadas expõem a interface da camada de transporte para as camadas acima delas.
  
4. **Drivers do sistema**. Normalmente, esses são clientes que usam uma interface de TDX (transporte data Extension) ou WSK (kernel do Winsock) para expor interfaces para aplicativos de modo de usuário. A interface WSK foi introduzida no Windows Server 2008 e no Windows&reg; vista, e é exposta por AFD. sys. A interface melhora o desempenho ao eliminar a alternância entre o modo de usuário e o modo kernel.
  
5. **Aplicativos de modo de usuário**. Normalmente, são soluções da Microsoft ou aplicativos personalizados.

A tabela a seguir fornece uma ilustração vertical das camadas da pilha de rede, incluindo exemplos de itens executados em cada camada.  

![Camadas da pilha de rede](../../media/Network-Subsystem/network-layers.jpg)

