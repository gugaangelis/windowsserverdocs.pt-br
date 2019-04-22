---
title: Etapa 4 verificar o Cluster
description: Este tópico faz parte do guia de implantação de acesso remoto em um Cluster no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b449197265befb7caf7d8d3a5b56accf5c52aef9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59821837"
---
# <a name="step-4-verify-the-cluster"></a>Etapa 4 verificar o Cluster

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como verificar se você configurou corretamente a implantação de cluster do DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>Para verificar o acesso aos recursos internos por meio do cluster  
  
1.  Conecte um computador cliente do DirectAccess à rede corporativa e obtenha a política de grupo.  
  
2.  Conecte o computador cliente à rede externa e tentar acessar os recursos internos.  
  
    Você deve conseguir acessar todos os recursos corporativos.  
  
3.  Teste a conectividade por meio de cada servidor no cluster como desativar ou desconexão da rede externa, todos, exceto um dos servidores de cluster. No computador cliente, tente acessar recursos corporativos. Repita o teste em um servidor de cluster diferente.  
  
    Você deve ser capaz de acessar todos os recursos corporativos por meio de cada servidor de cluster.  
  


