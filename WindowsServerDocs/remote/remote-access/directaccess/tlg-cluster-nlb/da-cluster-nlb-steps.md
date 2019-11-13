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

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

As etapas a seguir descrevem como configurar a infraestrutura de acesso remoto, configurar os servidores e clientes de acesso remoto e testar a conectividade do DirectAccess nas sub-redes Internet e HomeNet.  
  
Neste guia de laboratório de teste, você criará um cluster de acesso remoto habilitado para NLB (balanceamento de carga de rede) executando as seguintes etapas:  
  
-   [Etapa 1 conclua a configuração do DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Conclua todas as etapas no [Guia de laboratório de teste: demonstre a configuração do servidor único do DirectAccess com IPv4 e IPv6 mistos](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [Etapa 2: configurar o EDGE1](STEP-2-Configure-EDGE1.md). Configure a função de acesso remoto em EDGE1 para balanceamento de carga.  
  
-   [Etapa 3: instalar e configurar o EDGE2](STEP-3-Install-and-Configure-EDGE2.md). EDGE2 atua como o segundo servidor de acesso remoto em um cluster de acesso remoto.  
  
-   [Etapa 4: criar o cluster de acesso remoto com balanceamento de carga de rede](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md)-EDGE1 está configurado como o primeiro servidor em um cluster de acesso remoto. O EDGE2 é Unido ao cluster e o NLB está configurado para o cluster.  
  
-   [Etapa 5: testar a conectividade do DirectAccess da Internet e do cluster](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md). Após a conclusão da configuração do NLB e do cluster, você poderá testar a conectividade do cliente do DirectAccess por meio do cluster com balanceamento de carga.  
  
-   [Etapa 6: testar a conectividade do cliente do DirectAccess por trás de um dispositivo NAT](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md). Mova o computador cliente por trás de um dispositivo NAT para simular o teste de conectividade de cliente do DirectAccess por trás de um roteador doméstico.  
  
-   [Etapa 7: testar a conectividade ao retornar ao corpnet](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md). Verifique se o computador cliente ainda pode acessar recursos corporativos ao retornar ao corpnet.  
  
-   [Etapa 8: instantâneo da configuração](da-cluster-nlb-s8-snapshot.md). Depois de concluir o laboratório de teste, tire um instantâneo do cluster NLB de acesso remoto em funcionamento para que você possa retornar a ele mais tarde para testar cenários adicionais.  
  


