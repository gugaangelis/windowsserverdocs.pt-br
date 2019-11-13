---
title: Etapa 2 planejar a implantação do servidor RADIUS
description: Este tópico faz parte do guia implantar o acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2d6ad863-02a5-49b0-9aff-d189e78b2b80
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a991b312a0938a3809acd2b94c00aa678f5b41da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404395"
---
# <a name="step-2-plan-the-radius-server-deployment"></a>Etapa 2 planejar a implantação do servidor RADIUS

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Depois de implantar um único servidor de acesso remoto, planeje o servidor de autenticação OTP (senha de uso único).  
  
|Tarefa|Descrição|  
|----|--------|  
|2,1 planejar o servidor RADIUS|Para o servidor de autenticação OTP, o acesso remoto no Windows Server 2016 e no Windows Server 2012 dá suporte a qualquer servidor de OTP habilitado para RADIUS que dá suporte ao protocolo PAP.|  
  
## <a name="BKMK_1.1"></a>2,1 planejar o servidor RADIUS  
Observe o seguinte ao planejar um servidor RADIUS para autenticação OTP:  
  
-   Para a maioria dos tipos de implantações de OTP, você deve configurar o servidor de acesso remoto como um agente RADIUS. Para obter mais informações, consulte a documentação do fornecedor de OTP.  
  
-   Para todas as implantações de OTP, você deve sincronizar seus Active Directory usuários com o servidor RADIUS.  
  
-   O servidor RADIUS não precisa ser um membro do domínio.  
  
-   Ao implantar o servidor RADIUS, você configura um segredo compartilhado e o número da porta para o tráfego RADIUS. Anote esses detalhes; Eles são necessários quando você configura o servidor de acesso remoto.  
  
Você pode exibir um exemplo de guia de laboratório de teste que configura a autenticação de OTP com um servidor RSA SecurID no [Guia de laboratório de teste: demonstre o DirectAccess com autenticação OTP e RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  
  
  


