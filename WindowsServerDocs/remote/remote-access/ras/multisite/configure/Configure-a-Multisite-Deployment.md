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
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 25c0ce5d62268f64113ebc39345b2d50867bebf7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367123"
---
# <a name="configure-a-multisite-deployment"></a>Configurar uma implantação multissite

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

 O Windows Server 2016 combina a VPN do DirectAccess e do serviço de acesso remoto (RAS) em uma única função de acesso remoto. Esta visão geral fornece uma introdução às etapas de configuração necessárias para implantar uma única implantação de multissite de acesso remoto do Windows Server 2016 ou Windows Server 2012.  
  
-   Etapa 1: [Implante um único servidor DirectAccess com configurações avançadas](https://technet.microsoft.com/windows-server-docs/networking/remote-access/directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings). Instale e configure um único servidor de acesso remoto. A implantação multissite exige que você instale um único servidor antes de configurar uma implantação multissite.  
  
-   [Etapa 2: Configure a infraestrutura multissite @ no__t-0. Para uma implantação multissite, você deve configurar sites Active Directory adicionais e controladores de domínio. Grupos de segurança e objetos de Política de Grupo (GPOs) adicionais também são necessários se você não estiver usando GPOs configurados automaticamente.  
  
-   [Etapa 3: Configurar a implantação multissite @ no__t-0-instalar a função de acesso remoto em servidores de acesso remoto adicionais, habilitar a implantação multissite e configurar os servidores adicionais como pontos de entrada para a implantação.  
  
-   [Etapa 4: Verificar a implantação multissite @ no__t-0 
  


