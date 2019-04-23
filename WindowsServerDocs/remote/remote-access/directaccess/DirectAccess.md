---
title: DirectAccess
description: Você pode usar este tópico para obter uma visão geral do DirectAccess no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-da
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6b71d18e-1939-4fc0-bb42-29e0e5ffc8da
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 608d6b4dd3d5e894b28e767164b9370de9cb59ec
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59869567"
---
# <a name="directaccess"></a>DirectAccess

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter uma visão geral do DirectAccess, incluindo o servidor e sistemas operacionais que oferecem suporte ao DirectAccess, e links para documentação adicional do DirectAccess para Windows Server 2016.  
  
> [!NOTE]  
> Além deste tópico, a seguinte documentação do DirectAccess está disponível.  
>   
> -   [Caminhos de implantação do DirectAccess no Windows Server](DirectAccess-Deployment-Paths-in-Windows-Server.md)  
> -   [Pré-requisitos para implantação do DirectAccess](Prerequisites-for-Deploying-DirectAccess.md)  
> -   [Configurações sem suporte do DirectAccess](DirectAccess-Unsupported-Configurations.md)  
> -   [Guias de laboratório de teste do DirectAccess](DirectAccess-Test-Lab-Guides.md)  
> -   [Questões conhecidas do DirectAccess](DirectAccess-Known-Issues.md)  
> -   [Planejamento de capacidade do DirectAccess](DirectAccess-Capacity-Planning.md) 
> -   [Ingresso no domínio Offline do DirectAccess](DirectAccess-Offline-Domain-Join.md)  
> -   [Solucionar problemas do DirectAccess](Troubleshooting-DirectAccess.md)  
> -   [Implantar um único servidor DirectAccess usando o Assistente de Introdução](single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)  
> -   [Implantar um único servidor DirectAccess com configurações avançadas](single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
> -   [Adicionar DirectAccess a uma implantação existente do acesso remoto (VPN)](add-to-existing-vpn/Add-DirectAccess-to-an-Existing-Remote-Access-VPN-Deployment.md)  
  
O DirectAccess permite que a conectividade para usuários remotos a recursos de rede da organização sem a necessidade de conexões de rede Virtual privada (VPN) tradicionais. Com conexões do DirectAccess, computadores cliente remotos sempre estão conectados à sua organização – não é necessário para usuários remotos iniciar e parar de conexões, que é necessária para conexões VPN. Além disso, seus administradores de TI podem gerenciar computadores cliente do DirectAccess sempre que estiverem em execução e conectado à Internet.

>[!IMPORTANT]
>Não tente implantar o acesso remoto em uma máquina virtual \(VM\) no Microsoft Azure. Não há suporte para usar o acesso remoto no Microsoft Azure. Você não pode usar o acesso remoto em uma VM do Azure para implantar a VPN, DirectAccess ou qualquer outro recurso de acesso remoto no Windows Server 2016 ou versões anteriores do Windows Server. Para obter mais informações, consulte [suporte de software de servidor da Microsoft para máquinas virtuais do Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).
  
DirectAccess dá suporte apenas para clientes ingressados no domínio que incluem o suporte do sistema operacional para o DirectAccess.  
  
Os seguintes sistemas operacionais de servidor oferecem suporte ao DirectAccess.  
  
-   Você pode implantar todas as versões do Windows Server 2016 como um cliente DirectAccess ou um servidor DirectAccess.  
  
-   Você pode implantar todas as versões do Windows Server 2012 R2 como um cliente DirectAccess ou um servidor DirectAccess.  
  
-   Você pode implantar todas as versões do Windows Server 2012 como um cliente DirectAccess ou um servidor DirectAccess.  
  
-   Você pode implantar todas as versões do Windows Server 2008 R2 como um cliente DirectAccess ou um servidor DirectAccess.  
  
Os seguintes sistemas operacionais de cliente oferecem suporte ao DirectAccess.  
  
-   Windows 10 Enterprise  
  
-   Branch de manutenção de longo prazo do Enterprise 2015 Windows 10 (LTSB)  
  
-   Windows 8 e 8.1 Enterprise  
  
-   Windows 7 Ultimate  
  
-   Windows 7 Enterprise
