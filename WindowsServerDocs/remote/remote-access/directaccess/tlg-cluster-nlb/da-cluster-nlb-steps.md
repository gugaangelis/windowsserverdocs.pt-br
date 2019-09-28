---
title: Etapas para configurar o Test Lab DirectAccess Cluster-NLB
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess em um cluster com o NLB do Windows para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 108c63298ad3382f5ece790258f2d278bb03b78b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388396"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>Etapas para configurar o Test Lab DirectAccess Cluster-NLB

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

As etapas a seguir descrevem como configurar a infraestrutura de acesso remoto, configurar os servidores e clientes de acesso remoto e testar a conectividade do DirectAccess nas sub-redes Internet e HomeNet.  
  
Neste guia de laboratório de teste, você criará um cluster de acesso remoto habilitado para NLB (balanceamento de carga de rede) executando as seguintes etapas:  
  
-   [Etapa 1 conclua a configuração do DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Conclua todas as etapas no guia do laboratório do [Test: Demonstre a configuração do servidor único do DirectAccess com IPv4 misto e IPv6 @ no__t-0.  
  
-   [ETAPA 2: Configure EDGE1 @ no__t-0. Configure a função de acesso remoto em EDGE1 para balanceamento de carga.  
  
-   [ETAPA 3: Instale e configure o EDGE2 @ no__t-0. EDGE2 atua como o segundo servidor de acesso remoto em um cluster de acesso remoto.  
  
-   [ETAPA 4: Crie o cluster de acesso remoto com balanceamento de carga de rede @ no__t-0-EDGE1 está configurado como o primeiro servidor em um cluster de acesso remoto. O EDGE2 é Unido ao cluster e o NLB está configurado para o cluster.  
  
-   [ETAPA 5: Teste a conectividade do DirectAccess da Internet e por meio do cluster @ no__t-0. Após a conclusão da configuração do NLB e do cluster, você poderá testar a conectividade do cliente do DirectAccess por meio do cluster com balanceamento de carga.  
  
-   [ETAPA 6: Teste a conectividade do cliente do DirectAccess por trás de um dispositivo NAT @ no__t-0. Mova o computador cliente por trás de um dispositivo NAT para simular o teste de conectividade de cliente do DirectAccess por trás de um roteador doméstico.  
  
-   [ETAPA 7: Testar a conectividade ao retornar para o corpnet @ no__t-0. Verifique se o computador cliente ainda pode acessar recursos corporativos ao retornar ao corpnet.  
  
-   [ETAPA 8: Instantâneo da configuração @ no__t-0. Depois de concluir o laboratório de teste, tire um instantâneo do cluster NLB de acesso remoto em funcionamento para que você possa retornar a ele mais tarde para testar cenários adicionais.  
  


