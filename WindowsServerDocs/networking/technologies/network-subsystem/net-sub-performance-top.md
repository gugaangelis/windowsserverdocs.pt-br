---
title: Ajuste de desempenho do subsistema de rede
description: Este tópico faz parte do guia de ajuste de desempenho do subsistema de rede para o Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 45217fce-bfb9-47e8-9814-88ffdb3c7b7d
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: c0706c6ddbb678eacd3e609cfad3ccdda943fbd3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857407"
---
# <a name="network-subsystem-performance-tuning"></a>Ajuste de desempenho do subsistema de rede

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para uma visão geral do subsistema de rede e links para outros tópicos neste guia.

>[!NOTE]
>Além deste tópico, as seguintes seções deste guia fornecem recomendações de ajuste de desempenho para dispositivos de rede e a pilha de rede.
> - [Escolhendo um adaptador de rede](net-sub-choose-nic.md)
> - [Configurar a ordem das Interfaces de rede](net-sub-interface-metric.md)
> - [Adaptadores de rede de ajuste de desempenho](net-sub-performance-tuning-nics.md)
> - [Contadores de desempenho relacionados à rede](net-sub-performance-counters.md)
> - [Ferramentas de desempenho para cargas de trabalho de rede](net-sub-performance-tools.md)

O subsistema de rede, particularmente para cargas de trabalho com uso intensivo de rede, de ajuste de desempenho pode envolver cada camada da arquitetura de rede, que também é chamada para a pilha de rede. Essas camadas são amplamente divididas nas seções a seguir.

1. **Interface de rede**. Este é o nível mais baixo na pilha de rede e contém o driver de rede que se comunica diretamente com o adaptador de rede.

2. **Especificação de Interface de Driver (NDIS) de rede**. O NDIS expõe interfaces para o driver abaixo dele e para as camadas acima dele, como a pilha do protocolo.
  
3. **Pilha de protocolo**. A pilha do protocolo implementa os protocolos como TCP/IP e UDP/IP. Essas camadas expõem a interface de camada de transporte para camadas acima deles.
  
4. **Drivers do sistema**. Normalmente, são os clientes que usam uma extensão de dados de transporte (TDX) ou a interface WSK (Winsock Kernel) para expor interfaces para aplicativos de modo de usuário. A interface WSK foi introduzida no Windows Server 2008 e Windows&reg; Vista e ele é exposto pelo Afd. A interface melhora o desempenho, eliminando a alternância entre o modo de usuário e o modo de kernel.
  
5. **Modo de usuário**. Normalmente, são soluções da Microsoft ou aplicativos personalizados.

A tabela a seguir fornece uma ilustração vertical das camadas da pilha de rede, incluindo exemplos de itens que são executados em cada camada.  

![Camadas da pilha de rede](../../media/Network-Subsystem/network-layers.jpg)

