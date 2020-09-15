---
title: Implantar o Windows Server Update Services
description: O tópico WSUS (Windows Server Update Service) – uma visão geral do processo da implantação com links para quatro etapas para realizá-lo
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
ms.author: lizross
author: eross-msft
manager: mtillman
ms.date: 10/16/2017
ms.openlocfilehash: 78df9b93205e07e58310ad2077571b29b9ff5360
ms.sourcegitcommit: db2d46842c68813d043738d6523f13d8454fc972
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89624544"
---
# <a name="deploy-windows-server-update-services"></a>Implantar o Windows Server Update Services

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

O WSUS (Windows Server Update Services) permite que os administradores de Tecnologia da Informação implantem as atualizações mais recentes dos produtos da Microsoft. WSUS é uma função de servidor do Windows Server que pode ser instalada para gerenciar e distribuir atualizações. Um servidor do WSUS pode ser a fonte de atualização de outros servidores do WSUS na organização. O servidor WSUS que atua como fonte de atualização é chamado de servidor upstream.

Em uma implementação do WSUS, pelo menos um servidor do WSUS na rede precisa se conectar ao Microsoft Update para obter as informações de atualizações disponíveis. Você pode determinar, com base na segurança e configuração da rede, quantos outros servidores se conectam diretamente ao Microsoft Update.

Este guia fornece informações conceituais para o planejamento e a implantação do Serviço de Atualização do Windows Server.

-   [Planejar sua implantação do WSUS](../plan/plan-your-wsus-deployment.md)

-   [Etapa 1: Instalar a função de servidor do WSUS](1-install-the-wsus-server-role.md)

-   [Etapa 2: Configurar o WSUS](2-configure-wsus.md)

-   [Etapa 3: Aprovar e implantar atualizações no WSUS](3-approve-and-deploy-updates-in-wsus.md)

-   [Etapa 4: Definir as configurações da Política de Grupo para atualizações automáticas](4-configure-group-policy-settings-for-automatic-updates.md)
