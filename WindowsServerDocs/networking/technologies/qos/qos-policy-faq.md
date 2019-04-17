---
title: QoS perguntas frequentes
description: Este tópico fornece respostas para perguntas sobre a política de qualidade de serviço (QoS) no Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 74c97a14-b957-4568-b48e-8963a674fdb3
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: e63cd00a2016411e9f2c532c0e4c301dabdfd816
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
---
# <a name="qos-policy-frequently-asked-questions"></a>Perguntas frequentes sobre a política de QoS

>Aplica-se a: Windows Server (anual por canal), Windows Server 2016

A seguir é perguntas frequentes – e respostas a essas perguntas – para política de QoS.
  
1.  **Qual sistema operacional o controlador de domínio precisam estar em execução para usar a política de QoS?**
  
     Windows Server 2008, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 ou Windows Server 2016

2.  **Quais sistemas operacionais compatíveis com o aplicativo de política de QoS para o usuário ou computador?**

     Você pode aplicar políticas de QoS aos usuários ou computadores que executam o Windows Server 2016, Windows 10, Windows Server 2012 R2, Windows 8.1, Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows Server 2008 e Windows Vista.

3.  **QoS políticas se aplicam ao remetente ou receptor de tráfego?**

     Políticas de QoS devem ser aplicadas no computador de envio para afetar o tráfego de saída. Para afetar o tráfego de bidirecional dos dois computadores, políticas de QoS precisam ser aplicado aos dois computadores.

4.  **O que acontece se políticas de QoS conflitantes são implantadas no mesmo computador?**  
  
     Se várias políticas se aplicam, a política de QoS mais específica terá precedência. Por exemplo, uma política de estados de um host resolver (192.168.4.12) é aplicado em vez de um menos específica rede endereço (192.168.0.0/16). Se uma política de nível do computador e o nível de usuário tiver a mesma especificidade, a política de QoS de nível de usuário é aplicada em vez de diretiva de QoS do nível de computador. 

5.  **QoS política é habilitada por padrão?**

     Não, a política de QoS não está habilitada por padrão. Você deve criar políticas de QoS manualmente para habilitar QoS.  Para obter mais informações, consulte [gerenciar diretiva de QoS](qos-policy-manage.md).

Para o primeiro tópico neste guia, consulte [política de qualidade de serviço (QoS)](qos-policy-top.md).
