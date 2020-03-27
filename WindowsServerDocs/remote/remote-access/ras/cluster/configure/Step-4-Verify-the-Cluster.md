---
title: Etapa 4 verificar o cluster
description: Este tópico faz parte do guia implantar o acesso remoto em um cluster no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 16a31e5435bcb3f0fa3a332bba0e767beb913c08
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308201"
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
  


