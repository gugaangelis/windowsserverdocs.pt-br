---
title: Configurar uma implantação multissite
description: Este tópico faz parte do guia implantar vários servidores de acesso remoto em uma implantação multissite no Windows Server 2016.
manager: brianlic
ms.topic: article
ms.assetid: cb84920e-7cf5-4266-b071-d09e3d5e1f10
ms.author: lizross
author: eross-msft
ms.openlocfilehash: d5b175daf97d5957e9224003bccde9cbc1866295
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87937162"
---
# <a name="configure-a-multisite-deployment"></a>Configurar uma implantação multissite

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016

 O Windows Server 2016 combina a VPN do DirectAccess e do serviço de acesso remoto (RAS) em uma única função de acesso remoto. Esta visão geral fornece uma introdução às etapas de configuração necessárias para implantar uma única implantação de multissite de acesso remoto do Windows Server 2016 ou Windows Server 2012.

-   Etapa 1: [implantar um único servidor DirectAccess com configurações avançadas](../../../directaccess/single-server-advanced/deploy-a-single-directaccess-server-with-advanced-settings.md). Instale e configure um único servidor de acesso remoto. A implantação multissite exige que você instale um único servidor antes de configurar uma implantação multissite.

-   [Etapa 2: configurar a infraestrutura multissite](Step-2-Configure-the-Multisite-Infrastructure.md). Para uma implantação multissite, você deve configurar sites Active Directory adicionais e controladores de domínio. Grupos de segurança e objetos de Política de Grupo (GPOs) adicionais também são necessários se você não estiver usando GPOs configurados automaticamente.

-   [Etapa 3: configurar a implantação multissite](Step-3-Configure-the-Multisite-Deployment.md)– instale a função de acesso remoto em servidores de acesso remoto adicionais, habilite a implantação multissite e configure os servidores adicionais como pontos de entrada para a implantação.

-   [Etapa 4: verificar a implantação multissite](Step-4-Verify-the-Multisite-Deployment.md)

