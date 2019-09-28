---
title: Etapas para configurar o Test Lab
description: Este tópico faz parte do guia de laboratório de teste – demonstre o DirectAccess com autenticação OTP e RSA SecurID para Windows Server 2016
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a40183c-afd1-43ca-b306-05745640a37d
ms.author: pashort
author: shortpatti
ms.openlocfilehash: ef5ce37983b8565fab8287eeaae7423be0c269f6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404729"
---
# <a name="steps-for-configuring-the-test-lab"></a>Etapas para configurar o Test Lab

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

As etapas a seguir descrevem como configurar a infraestrutura de acesso remoto, configurar o servidor de acesso remoto e o cliente e testar a conectividade do DirectAccess nas sub-redes HomeNet e Internet.  
  
Neste guia de laboratório de teste, você criará um acesso remoto com o ambiente de OTP executando as seguintes etapas:  
  
-   [ETAPA 1: Conclua a configuração do DirectAccess @ no__t-0. Conclua todas as etapas no guia do laboratório do [Test: Demonstre a configuração do servidor único do DirectAccess com IPv4 misto e IPv6 @ no__t-0.  
  
-   [ETAPA 2: Configure APP1 @ no__t-0. Configure o APP1 com modelos de certificado de OTP para uso pelo EDGE1.  
  
-   [ETAPA 3: Configure DC1 @ no__t-0. Verifique o nome principal do usuário definido em DC1.  
  
-   [ETAPA 7: Instale e configure o RSA @ no__t-0. Instale e configure o RSA, um servidor RADIUS e de OTP e configure o EDGE1 para OTP.  
  
-   [ETAPA 11: Verifique a integridade da OTP em EDGE1 @ no__t-0. Verifique se o status de OTP está íntegro no servidor de acesso remoto.  
  
-   [ETAPA 8: Teste a conectividade do DirectAccess da sub-rede HomeNet @ no__t-0. Testar a funcionalidade de OTP do DirectAccess por trás de um dispositivo NAT.  
  
-   [ETAPA 10: Teste a conectividade do DirectAccess da Internet @ no__t-0. Testar a conectividade de cliente do DirectAccess da Internet.  
  
-   [ETAPA 12: Instantâneo da configuração @ no__t-0. Depois de concluir o laboratório de teste, tire um instantâneo do DirectAccess em funcionamento com a configuração de OTP para que você possa retornar a ele mais tarde para testar cenários adicionais.  
  


