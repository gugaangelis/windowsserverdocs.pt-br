---
title: Planejar acesso remoto com autenticação OTP
description: Este tópico faz parte do guia implantar o acesso remoto com autenticação OTP no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 762bc463-eead-46ac-8b90-32355743c27c
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ddcfa2898f4b90bf724a547bb16244cfad4ab3ff
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858189"
---
# <a name="plan-remote-access-with-otp-authentication"></a>Planejar acesso remoto com autenticação OTP

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

 O Windows Server 2016 e o Windows Server 2012 combinam o DirectAccess e o serviço de roteamento e acesso remoto (RRAS) para uma única função de acesso remoto. Esta visão geral fornece uma introdução às etapas de configuração necessárias para implantar uma única implantação de multissite de acesso remoto do Windows Server 2016 ou Windows Server 2012.  
  
  
-  Etapa 1: [implantar um único servidor DirectAccess com configurações avançadas](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Esta etapa inclui o planejamento da infraestrutura necessária para implantar um único servidor. Ele inclui o planejamento de configurações de rede e servidor, requisitos de certificado, configurações de DNS, implantação de servidor de local de rede, servidores de gerenciamento do DirectAccess, configurações de Active Directory e objetos de Política de Grupo (GPOs).  
  
-   [Etapa 2: planejar a implantação do servidor RADIUS](Step-2-Plan-the-RADIUS-Server-Deployment.md)  
  
-   [Etapa 3: planejar a implantação de certificado de OTP](Step-3-Plan-OTP-Certificate-Deployment.md)  
  
-   [Etapa 4: planejar a OTP no servidor de acesso remoto](Step-4-Plan-for-OTP-on-the-Remote-Access-Server.md)  
  
Depois de concluir essas etapas de planejamento, consulte [Configurar o acesso remoto com autenticação OTP](https://technet.microsoft.com/windows-server-docs/networking/remote-access/ras/otp/configure/configure-ra-with-otp-authentication). Para obter informações sobre como configurar uma implantação multissite como uma prova de conceito em um ambiente de laboratório, consulte [Guia de laboratório de teste: demonstre o DirectAccess com autenticação de OTP e RSA SecurID](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/tlg-otp-securid/test-lab-guide-demonstrate-directaccess-with-otp-authentication-and-rsa-securid).  
  


