---
title: Etapas para configurar o Test Lab
description: Este tópico faz parte do guia de laboratório de teste – demonstre uma implantação multissite do DirectAccess para o Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dc7205b4-a822-4038-ab67-ec0a870737f2
ms.author: pashort
author: shortpatti
ms.openlocfilehash: dd8b8864dff98e51bf55aad9307523df4a0c30bf
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404698"
---
# <a name="steps-for-configuring-the-test-lab"></a>Etapas para configurar o Test Lab

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

As etapas a seguir descrevem como configurar a infraestrutura de acesso remoto, configurar os servidores de acesso remoto e os clientes e testar a conectividade do DirectAccess nas sub-redes Internet e HomeNet.  
  
Neste guia de laboratório de teste, você criará uma implantação de acesso remoto multissite executando as seguintes etapas:  
  
-   [ETAPA 1: Conclua a configuração de base @ no__t-0. Conclua todas as etapas no guia do laboratório do [Test: Demonstre a configuração do servidor único do DirectAccess com IPv4 misto e IPv6 @ no__t-0.  
  
-   [ETAPA 2: Instale e configure o ROUTER1 @ no__t-0. O ROUTER1 fornece a funcionalidade de roteamento e encaminhamento entre as sub-redes corpnet e 2-corpnet.  
  
-   [ETAPA 3: Instale e configure o CLIENT2 @ no__t-0. CLIENT2 é um computador cliente do Windows 7 que é usado para demonstrar a compatibilidade com versões anteriores de uma implantação de acesso remoto do Windows Server 2016, do Windows Server 2012 R2 ou do Windows Server 2012.  
  
-   [ETAPA 4: Configure APP1 @ no__t-0. Configure o APP1 com ROUTER1 como o gateway padrão e 2-DC1 como o servidor DNS alternativo.  
  
-   [ETAPA 5: Configure DC1 @ no__t-0. Configure o DC1 com um site Active Directory adicional e grupos de segurança adicionais para computadores cliente do Windows 7.  
  
-   [ETAPA 6: Instale e configure 2-DC1 @ no__t-0. Em uma implantação multissite, você tem dois ou mais domínios e sites. 2-DC1 fornece o controlador de domínio e os serviços DNS para o domínio corp2.corp.contoso.com.  
  
-   [ETAPA 7: Instalar e configurar o 2-APP1 @ no__t-0. 2-APP1 é um servidor de arquivos e Web na rede de 2 corpnet.  
  
-   [ETAPA 8: Configure INET1 @ no__t-0. INET1 simula a Internet neste guia de laboratório de teste. Você deve configurar uma entrada DNS que seja resolvida para o endereço IP público de 2-EDGE1.  
  
-   [ETAPA 9: Configure EDGE1 @ no__t-0. Configure o servidor DNS 2-corpnet e o roteamento no EDGE1.  
  
-   [ETAPA 10: Instale e configure o 2-EDGE1 @ no__t-0. Dois servidores de acesso remoto são necessários em uma implantação multissite. 2-EDGE1 fornece serviços de acesso remoto para o segundo domínio.  
  
-   [ETAPA 11: Configure a implantação multissite @ no__t-0. Depois de configurar os servidores de acesso remoto, você pode configurar a implantação multissite.  
  
-   [ETAPA 12: Teste a conectividade do DirectAccess @ no__t-0. Teste a conectividade do DirectAccess de ambos os computadores cliente da sub-rede da Internet por meio de EDGE1 e 2-EDGE1.  
  
-   [ETAPA 13: Teste a conectividade do DirectAccess por trás de um dispositivo NAT @ no__t-0. Teste a conectividade do DirectAccess por trás de um dispositivo NAT.  
  
-   [ETAPA 14: Instantâneo da configuração @ no__t-0. Depois de concluir o laboratório de teste, tire um instantâneo da implantação de multissite de acesso remoto em funcionamento para que você possa retornar a ele mais tarde para testar cenários adicionais.  
  


