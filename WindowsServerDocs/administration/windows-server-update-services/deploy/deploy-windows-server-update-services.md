---
title: Implantar o Windows Server Update Services
description: O tópico WSUS (Windows Server Update Service) – uma visão geral do processo da implantação com links para quatro etapas para realizá-lo
ms.topic: get-started-article
ms.assetid: 2708f6b2-4252-4b8f-9b7e-84c9b4222075
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8011da301c1b4733692910b1b3f126d0f59f9307
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87881155"
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
