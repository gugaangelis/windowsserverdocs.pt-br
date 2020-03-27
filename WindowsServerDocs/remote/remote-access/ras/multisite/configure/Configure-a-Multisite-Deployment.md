---
title: Configurar uma implantação multissite
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 4a0229d5605271876f89e8e0ae75f8612e3f5762
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314013"
---
# <a name="configure-a-multisite-deployment"></a>Configurar uma implantação multissite

>Aplicável ao: Windows Server (canal semestral), Windows Server 2016

 O Windows Server 2016 combina a VPN do DirectAccess e do serviço de acesso remoto (RAS) em uma única função de acesso remoto. Esta visão geral fornece uma introdução às etapas de configuração necessárias para implantar uma única implantação de multissite de acesso remoto do Windows Server 2016 ou Windows Server 2012.  
  
-   Etapa 1: [implantar um único servidor DirectAccess com configurações avançadas](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Instale e configure um único servidor de acesso remoto. A implantação multissite exige que você instale um único servidor antes de configurar uma implantação multissite.  
  
-   [Etapa 2: configurar a infraestrutura multissite](Step-2-Configure-the-Multisite-Infrastructure.md). Para uma implantação multissite, você deve configurar sites Active Directory adicionais e controladores de domínio. Grupos de segurança e objetos de Política de Grupo (GPOs) adicionais também são necessários se você não estiver usando GPOs configurados automaticamente.  
  
-   [Etapa 3: configurar a implantação multissite](Step-3-Configure-the-Multisite-Deployment.md)– instale a função de acesso remoto em servidores de acesso remoto adicionais, habilite a implantação multissite e configure os servidores adicionais como pontos de entrada para a implantação.  
  
-   [Etapa 4: verificar a implantação multissite](Step-4-Verify-the-Multisite-Deployment.md) 
  


