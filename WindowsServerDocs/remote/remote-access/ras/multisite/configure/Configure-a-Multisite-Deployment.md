---
title: Configurar uma implantação multissite
description: Este tópico faz parte do guia de implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b602855db271348ac48ee0a5691424a7321c7370
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849747"
---
# <a name="configure-a-multisite-deployment"></a>Configurar uma implantação multissite

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016

 Windows Server 2016 combina o DirectAccess e o serviço de acesso remoto (RAS) VPN em uma única função de acesso remoto. Esta visão geral fornece uma introdução às etapas de configuração necessárias para implantar uma única implantação multissite do Windows Server 2016 ou o acesso remoto do Windows Server 2012.  
  
-   Etapa 1: [Implantar um único servidor DirectAccess com configurações avançadas](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Instalar e configurar um único servidor de acesso remoto. Implantação multissite requer a instalação de um único servidor antes de configurar uma implantação multissite.  
  
-   [Etapa 2: Configurar a infraestrutura de multissite](Step-2-Configure-the-Multisite-Infrastructure.md). Para uma implantação multissite, você deve configurar sites adicionais do Active Directory e controladores de domínio. Grupos de segurança adicional e objetos de diretiva de grupo (GPOs) também são necessários se você não estiver usando GPOs configurados automaticamente.  
  
-   [Etapa 3: Configurar a implantação multissite](Step-3-Configure-the-Multisite-Deployment.md)-instalar a função de acesso remoto em servidores de acesso remoto adicionais, habilitar a implantação multissite e configurar os servidores adicionais como pontos de entrada para a implantação.  
  
-   [Etapa 4: Verificar a implantação multissite](Step-4-Verify-the-Multisite-Deployment.md) 
  


