---
title: Perguntas frequentes sobre o QoS
description: Este tópico fornece respostas para perguntas sobre a política de qualidade de serviço (QoS) no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f9fb54cc3bda9a259188ae02fb2ef2836dd8718d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833747"
---
# <a name="qos-policy-frequently-asked-questions"></a>Perguntas frequentes sobre a política de QoS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

A seguir está perguntas frequentes – e respostas para essas perguntas – para a política de QoS.
  
1.  **Sistema operacional que o meu controlador de domínio precisa estar em execução para usar a política de QoS?**
  
     Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2008

2.  **Quais sistemas operacionais dão suporte a aplicação de diretiva de QoS para o usuário ou computador?**

     Você pode aplicar políticas de QoS a usuários ou computadores que executam o Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista.

3.  **As políticas de QoS se aplica ao remetente ou receptor de tráfego?**

     As políticas de QoS devem ser aplicadas no computador de envio para afetar o respectivo tráfego de saída. Para afetar o tráfego bidirecional de dois computadores, políticas de QoS precisam ser aplicado a ambos os computadores.

4.  **O que acontece se as políticas de QoS conflitantes forem implantadas no mesmo computador?**  
  
     Se várias políticas se aplicam, a política de QoS mais específica tem precedência. Por exemplo, uma política que declara um endereço de host (192.168.4.12) é aplicada, em vez de um endereço de rede (192.168.0.0/16) menos específico. Se uma política de nível de computador e o nível de usuário tiver a mesma especificidade, a política de QoS de nível de usuário é aplicada, em vez da política de QoS do nível do computador. 

5.  **Política de QoS está habilitada por padrão?**

     Não, a política de QoS não está habilitada por padrão. Você deve criar políticas de QoS manualmente para habilitar a QoS.  Para obter mais informações, consulte [Gerenciar política de QoS](qos-policy-manage.md).

Para o primeiro tópico deste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).
