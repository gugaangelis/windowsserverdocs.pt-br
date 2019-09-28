---
title: Perguntas frequentes sobre QoS
description: Este tópico fornece respostas para perguntas sobre a política de qualidade de serviço (QoS) no Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 1bde6c859284fbd1756517d7b08ec96816bb2bb5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71395962"
---
# <a name="qos-policy-frequently-asked-questions"></a>Perguntas frequentes sobre a política de QoS

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

A seguir, são perguntas frequentes – e respostas para essas perguntas – para a política de QoS.
  
1.  **Qual sistema operacional meu controlador de domínio precisa estar em execução para usar a política de QoS?**
  
     Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008

2.  **Quais sistemas operacionais dão suporte à aplicação de política de QoS para o usuário ou computador?**

     Você pode aplicar políticas de QoS a usuários ou computadores que executam o Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista.

3.  **As políticas de QoS se aplicam ao remetente ou ao destinatário do tráfego?**

     As políticas de QoS devem ser aplicadas no computador de envio para afetar o tráfego de saída. Para afetar o tráfego bidirecional de dois computadores, as políticas de QoS precisam ser aplicadas a ambos os computadores.

4.  **O que acontece se as políticas de QoS conflitantes forem implantadas no mesmo computador?**  
  
     Se várias políticas se aplicarem, a política de QoS mais específica terá precedência. Por exemplo, uma política que declara um endereço de host (192.168.4.12) é aplicada em vez de um endereço de rede menos específico (192.168.0.0/16). Se uma política de nível de computador e de usuário tiver a mesma especificidade, a política de QoS no nível de usuário será aplicada em vez da política de QoS no nível do computador. 

5.  **A política de QoS está habilitada por padrão?**

     Não, a política de QoS não está habilitada por padrão. Você deve criar políticas de QoS manualmente para habilitar a QoS.  Para obter mais informações, consulte [gerenciar política de QoS](qos-policy-manage.md).

Para o primeiro tópico deste guia, consulte [política de QoS (qualidade de serviço)](qos-policy-top.md).
