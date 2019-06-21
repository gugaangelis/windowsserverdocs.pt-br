---
title: Etapa 2 planejar a implantação do servidor RADIUS
description: Este tópico faz parte do guia de implantação de acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: f74a83c3962c7accd76fbf07307216742ada863d
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280829"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>Etapa 2 planejar a implantação do servidor RADIUS

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Depois de implantar um único servidor de acesso remoto, planeje o servidor de autenticação de senha única (OTP).  
  
|Tarefa|Descrição|  
|----|--------|  
|2.1 planejar o servidor RADIUS|Para o servidor de autenticação de OTP, o acesso remoto no Windows Server 2016 e Windows Server 2012 dá suporte a qualquer servidor OTP habilitado para RADIUS que suporte o protocolo de autenticação de senha (PAP).|  
  
## <a name="BKMK_1.1"></a>2.1 planejar o servidor RADIUS  
Observe o seguinte ao planejar um servidor RADIUS para autenticação de OTP:  
  
-   Para a maioria dos tipos de implantações de OTP, você deve configurar o servidor de acesso remoto como um agente de RADIUS. Para obter mais informações, consulte a documentação do fornecedor OTP.  
  
-   Para todas as implantações de OTP, você deve sincronizar os usuários do Active Directory com o servidor RADIUS.  
  
-   O servidor RADIUS não precisa ser um membro do domínio.  
  
-   Quando você implanta o servidor RADIUS, configure um segredo compartilhado e o número da porta para o tráfego RADIUS. Anote esses detalhes. elas são necessárias quando você configura o servidor de acesso remoto.  
  
Você pode exibir um guia de laboratório de teste de exemplo que configura a autenticação de OTP com um servidor do RSA SecurID na [Test Lab Guide: Demonstrar o DirectAccess com autenticação OTP e SecurID RSA](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  
  
  


