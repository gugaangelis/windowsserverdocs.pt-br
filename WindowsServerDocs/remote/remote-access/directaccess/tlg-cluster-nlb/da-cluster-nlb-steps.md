---
title: Etapas para configurar o Test Lab DirectAccess Cluster-NLB
description: Este tópico faz parte do guia de laboratório de teste - demonstração do DirectAccess em um Cluster com Windows NLB para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e508d3ee-ffa6-463f-a3dd-9e35e745c005
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3a243debf79d2c9d12de511153b74f23c5a44cce
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880737"
---
# <a name="steps-for-configuring-the-directaccess-cluster-nlb-test-lab"></a>Etapas para configurar o Test Lab DirectAccess Cluster-NLB

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

As etapas a seguir descrevem como configurar a infraestrutura de acesso remoto, configurar os servidores de acesso remoto e clientes e testar a conectividade do DirectAccess em sub-redes da rede doméstica e de Internet.  
  
Neste teste o guia de laboratório, você criará uma rede NLB Balanceamento de carga () habilitada cluster de acesso remoto executando as seguintes etapas:  
  
-   [Etapa 1 concluir a configuração do DirectAccess](STEP-1-Complete-the-DirectAccess-Configuration.md). Conclua todas as etapas a [guia do laboratório de teste: Demonstrar a configuração de servidor único do DirectAccess com misto de IPv4 e IPv6](https://go.microsoft.com/fwlink/p/?LinkId=237004).  
  
-   [ETAPA 2: Configurar EDGE1](STEP-2-Configure-EDGE1.md). Configure a função acesso remoto em EDGE1 para balanceamento de carga.  
  
-   [ETAPA 3: Instalar e configurar o EDGE2](STEP-3-Install-and-Configure-EDGE2.md). EDGE2 atua como o segundo servidor de acesso remoto em um cluster de acesso remoto.  
  
-   [ETAPA 4: Criar o cluster de acesso remoto do balanceamento de carga de rede](STEP-4-Create-the-Network-Load-Balanced-Remote-Access-Cluster.md)-EDGE1 é configurado como o primeiro servidor em um cluster de acesso remoto. EDGE2 está associado ao cluster e NLB é configurado para o cluster.  
  
-   [ETAPA 5: Testar a conectividade do DirectAccess da Internet e por meio do cluster](STEP-5-Test-DirectAccess-Connectivity-from-the-Internet-and-Through-the-Cluster.md). Após a conclusão da configuração do NLB e cluster, você pode testar a conectividade de cliente DirectAccess por meio do cluster com balanceamento de carga.  
  
-   [ETAPA 6: Testar a conectividade de cliente do DirectAccess por trás de um dispositivo NAT](STEP-6-Test-DirectAccess-Client-Connectivity-from-Behind-a-NAT-Device.md). Mova o computador cliente atrás de um dispositivo NAT para simular o teste conectividade de cliente do DirectAccess por trás de um roteador doméstico.  
  
-   [ETAPA 7: Testar a conectividade ao retornar para a rede corporativa](STEP-7-Test-Connectivity-When-Returning-to-the-Corpnet.md). Certifique-se de que o computador cliente ainda pode acessar recursos corporativos ao retornar para a rede corporativa.  
  
-   [ETAPA 8: Instantâneo da configuração](da-cluster-nlb-s8-snapshot.md). Depois de concluir o laboratório de teste, tire um instantâneo de trabalho de cluster NLB de acesso remoto para que você possa retornar a ele mais tarde para testar cenários adicionais.  
  


