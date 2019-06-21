---
title: Planejar acesso remoto com autenticação OTP
description: Este tópico faz parte do guia de implantação de acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 328e092dff23495203ee21fccbace1f7f36918d2
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280820"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Planejar acesso remoto com autenticação OTP

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

 Windows Server 2016 e Windows Server 2012 combinam o DirectAccess e o roteamento e acesso remoto (RRAS) VPN em uma única função de acesso remoto. Esta visão geral fornece uma introdução às etapas de configuração necessárias para implantar uma única implantação multissite do Windows Server 2016 ou o acesso remoto do Windows Server 2012.  
  
  
-  Etapa 1: [Implantar um único servidor DirectAccess com configurações avançadas](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Esta etapa inclui o planejamento da infraestrutura necessária para implantar um único servidor. Ele inclui o planejamento da rede e as configurações do servidor, requisitos de certificado, as configurações de DNS, implantação de servidor de local de rede, servidores de gerenciamento do DirectAccess, configurações do Active Directory e objetos de diretiva de grupo (GPOs).  
  
-   [Etapa 2: Planejar a implantação do servidor RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [Etapa 3: Planejar a implantação de certificado OTP](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [Etapa 4: Planeje de OTP no servidor de acesso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
Depois de concluir essas etapas de planejamento, consulte [configurar o acesso remoto com autenticação OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication). Para obter informações sobre como configurar uma implantação multissite, como uma prova de conceito em um ambiente de laboratório, consulte [guia do laboratório de teste: Demonstrar o DirectAccess com autenticação OTP e SecurID RSA](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  


