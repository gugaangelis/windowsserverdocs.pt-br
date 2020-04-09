---
title: Etapa 4 verificar o cluster
description: Este tópico faz parte do guia implantar o acesso remoto em um cluster no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: lizross
author: eross-msft
ms.openlocfilehash: c31db158856eb3c3881a36c29e40d1227116201f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855259"
---
# <a name="step-4-verify-the-cluster"></a>Etapa 4 verificar o cluster

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Este tópico descreve como verificar se você configurou corretamente sua implantação de cluster do DirectAccess.  
  
### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>Para verificar o acesso a recursos internos por meio do cluster  
  
1.  Conecte um computador cliente do DirectAccess à rede corporativa e obtenha a política de grupo.  
  
2.  Conecte o computador cliente à rede externa e tentar acessar os recursos internos.  
  
    Você deve conseguir acessar todos os recursos corporativos.  
  
3.  Teste a conectividade por meio de cada servidor no cluster desativando ou desconectando da rede externa, tudo menos um dos servidores de cluster. No computador cliente, tente acessar os recursos corporativos. Repita o teste em um servidor de cluster diferente.  
  
    Você deve ser capaz de acessar todos os recursos corporativos por meio de cada servidor de cluster.  
  


