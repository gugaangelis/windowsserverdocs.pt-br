---
title: Implantar o Windows Server Update Services
description: Tópico Windows Server Update Service (WSUS)-uma visão geral do processo de implantação com links para as quatro etapas para realizá-lo
ms.prod: windows-server
ms.reviewer: na
ms.technology: manage-wsus
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e3e6bcd5f90d1a7df2a35dda45b4bf8951940815
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361683"
---
# <a name="deploy-windows-server-update-services"></a>Implantar o Windows Server Update Services

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O WSUS (Windows Server Update Services) permite que os administradores de Tecnologia da Informação implantem as atualizações mais recentes dos produtos da Microsoft. WSUS é uma função de servidor do Windows Server que pode ser instalada para gerenciar e distribuir atualizações. Um servidor do WSUS pode ser a fonte de atualização de outros servidores do WSUS na organização. O servidor WSUS que atua como fonte de atualização é chamado de servidor upstream.  

Em uma implementação do WSUS, pelo menos um servidor do WSUS na rede precisa se conectar ao Microsoft Update para obter as informações de atualizações disponíveis. Você pode determinar, com base em segurança e configuração de rede, quantos outros servidores se conectam diretamente a Microsoft Update.  

Este guia fornece informações conceituais para o planejamento e a implantação do serviço de atualização do Windows Server.  

-   [Planejar sua implantação do WSUS](../plan/plan-your-wsus-deployment.md)  

-   [Etapa 1: Instalar a função de servidor do WSUS](1-install-the-wsus-server-role.md)  

-   [Etapa 2: Configurar o WSUS](2-configure-wsus.md)  

-   [Etapa 3: Aprovar e implantar atualizações no WSUS](3-approve-and-deploy-updates-in-wsus.md)  

-   [Etapa 4: Definir as configurações da Política de Grupo para atualizações automáticas](4-configure-group-policy-settings-for-automatic-updates.md)  
