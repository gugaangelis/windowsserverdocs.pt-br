---
title: DirectAccess
description: Você pode usar este tópico para obter uma breve visão geral do DirectAccess no Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-da
ms.topic: article
ms.assetid: 6b71d18e-1939-4fc0-bb42-29e0e5ffc8da
ms.author: lizross
author: eross-msft
ms.openlocfilehash: facd4fc6821d8f1dc927e1247aa3e2537e027f2f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857459"
---
# <a name="directaccess"></a>DirectAccess

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

Você pode usar este tópico para obter uma breve visão geral do DirectAccess, incluindo os sistemas operacionais de servidor e cliente que dão suporte ao DirectAccess e links para a documentação adicional do DirectAccess para o Windows Server 2016.  
  
> [!NOTE]  
> Além deste tópico, a seguinte documentação do DirectAccess está disponível.  
>   
> -   [Caminhos de implantação do DirectAccess no Windows Server](DirectAccess-Deployment-Paths-in-Windows-Server.md)  
> -   [Pré-requisitos para implantação do DirectAccess](Prerequisites-for-Deploying-DirectAccess.md)  
> -   [Configurações do DirectAccess sem suporte](DirectAccess-Unsupported-Configurations.md)  
> -   [Guias de laboratório de teste do DirectAccess](DirectAccess-Test-Lab-Guides.md)  
> -   [Problemas conhecidos do DirectAccess](DirectAccess-Known-Issues.md)  
> -   [Planejamento de capacidade do DirectAccess](DirectAccess-Capacity-Planning.md) 
> -   [Ingresso no domínio offline do DirectAccess](DirectAccess-Offline-Domain-Join.md)  
> -   [Como solucionar problemas do DirectAccess](Troubleshooting-DirectAccess.md)  
> -   [Implantar um único servidor DirectAccess usando o assistente de Introdução](single-server-wizard/Deploy-a-Single-DirectAccess-Server-Using-the-Getting-Started-Wizard.md)  
> -   [Implantar um único servidor do DirectAccess com configurações avançadas](single-server-advanced/Deploy-a-Single-DirectAccess-Server-with-Advanced-Settings.md)  
> -   [Adicionar o DirectAccess a uma implantação de acesso remoto existente (VPN)](add-to-existing-vpn/Add-DirectAccess-to-an-Existing-Remote-Access-VPN-Deployment.md)  
  
O DirectAccess permite a conectividade para usuários remotos para recursos de rede da organização sem a necessidade de conexões de VPN (rede virtual privada) tradicionais. Com as conexões do DirectAccess, os computadores cliente remotos são sempre conectados à sua organização – não há necessidade de que os usuários remotos iniciem e parem conexões, como é necessário com conexões VPN. Além disso, os administradores de ti podem gerenciar computadores cliente do DirectAccess sempre que estiverem em execução e conectados à Internet.

>[!IMPORTANT]
>Não tente implantar o acesso remoto em uma máquina virtual \(\) VM no Microsoft Azure. Não há suporte para o uso do acesso remoto no Microsoft Azure. Você não pode usar o acesso remoto em uma VM do Azure para implantar VPN, DirectAccess ou qualquer outro recurso de acesso remoto no Windows Server 2016 ou versões anteriores do Windows Server. Para obter mais informações, consulte [suporte de software de servidor da Microsoft para máquinas virtuais Microsoft Azure](https://support.microsoft.com/help/2721672/microsoft-server-software-support-for-microsoft-azure-virtual-machines).
  
O DirectAccess fornece suporte apenas para clientes ingressados no domínio que incluem suporte ao sistema operacional para o DirectAccess.  
  
Os sistemas operacionais de servidor a seguir dão suporte ao DirectAccess.  
  
-   Você pode implantar todas as versões do Windows Server 2016 como um cliente DirectAccess ou um servidor DirectAccess.  
  
-   Você pode implantar todas as versões do Windows Server 2012 R2 como um cliente DirectAccess ou um servidor DirectAccess.  
  
-   Você pode implantar todas as versões do Windows Server 2012 como um cliente DirectAccess ou um servidor DirectAccess.  
  
-   Você pode implantar todas as versões do Windows Server 2008 R2 como um cliente DirectAccess ou um servidor DirectAccess.  
  
Os sistemas operacionais cliente a seguir dão suporte ao DirectAccess.  
  
-   Windows 10 Enterprise  
  
-   Windows 10 Enterprise 2015 Branch de Manutenção em Longo Prazo (LTSB)  
  
-   Windows 8 e 8,1 Enterprise  
  
-   Windows 7 Ultimate  
  
-   Windows 7 Enterprise
