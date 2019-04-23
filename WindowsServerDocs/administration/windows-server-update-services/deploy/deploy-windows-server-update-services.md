---
title: Implantar o Windows Server Update Services
description: Tópico do Windows Server Update Service (WSUS) - uma visão geral do processo de implantação com links para as quatro etapas para fazer isso
ms.prod: windows-server-threshold
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 51972ad352f6530c8ee2aa84aec57b62784da728
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873177"
---
# <a name="deploy-windows-server-update-services"></a>Implantar o Windows Server Update Services

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O WSUS (Windows Server Update Services) permite que os administradores de Tecnologia da Informação implantem as atualizações mais recentes dos produtos da Microsoft. WSUS é uma função de servidor do Windows Server que pode ser instalada para gerenciar e distribuir atualizações. Um servidor do WSUS pode ser a fonte de atualização de outros servidores do WSUS na organização. O servidor WSUS que atua como fonte de atualização é chamado de servidor upstream.  

Em uma implementação do WSUS, pelo menos um servidor do WSUS na rede precisa se conectar ao Microsoft Update para obter as informações de atualizações disponíveis. Você pode determinar, com base na configuração e segurança de rede, quantos outros servidores se conectam diretamente ao Microsoft Update.  

Este guia fornece informações conceituais para planejar e implantar o Windows Server Update Services.  

-   [Planejar a implantação do WSUS](../plan/plan-your-wsus-deployment.md)  

-   [Etapa 1: Instalar a função de servidor do WSUS](1-install-the-wsus-server-role.md)  

-   [Etapa 2: Configurar o WSUS](2-configure-wsus.md)  

-   [Etapa 3: Aprovar e implantar atualizações no WSUS](3-approve-and-deploy-updates-in-wsus.md)  

-   [Etapa 4: Definir configurações de política de grupo para atualizações automáticas](4-configure-group-policy-settings-for-automatic-updates.md)  
