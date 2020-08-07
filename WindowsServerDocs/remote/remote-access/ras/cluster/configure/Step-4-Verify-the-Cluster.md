---
title: Etapa 4 verificar o cluster
description: Este tópico faz parte do guia implantar o acesso remoto em um cluster no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: f22dcf10-b453-4664-a9ef-e40e95c72f63
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4fa053519116ddadbe1469223001a33384aebe91
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87963892"
---
# <a name="step-4-verify-the-cluster"></a>Etapa 4 verificar o cluster

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

Este tópico descreve como verificar se você configurou corretamente sua implantação de cluster do DirectAccess.

### <a name="to-verify-access-to-internal-resources-through-the-cluster"></a>Para verificar o acesso a recursos internos por meio do cluster

1.  Conecte um computador cliente do DirectAccess à rede corporativa e obtenha a política de grupo.

2.  Conecte o computador cliente à rede externa e tentar acessar os recursos internos.

    Você deve conseguir acessar todos os recursos corporativos.

3.  Teste a conectividade por meio de cada servidor no cluster desativando ou desconectando da rede externa, tudo menos um dos servidores de cluster. No computador cliente, tente acessar os recursos corporativos. Repita o teste em um servidor de cluster diferente.

    Você deve ser capaz de acessar todos os recursos corporativos por meio de cada servidor de cluster.



