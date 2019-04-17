---
title: Use o Windows Admin Center para gerenciar atualizações do sistema operacional com o gerenciamento de atualização do Azure
description: Use o Windows Admin Center (Project Honolulu) para configurar o gerenciamento de atualização do Azure para gerenciar o sistema operacional atualizações.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 5ba81968f8baa81176ad646fb2a97961ddc49fda
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296844"
---
# Use o Windows Admin Center para gerenciar atualizações do sistema operacional com o gerenciamento de atualização do Azure

[Saiba mais sobre a integração do Azure com o Windows Admin Center.](../plan/azure-integration-options.md)

Gerenciamento de atualização do Azure é uma solução de automação do Azure que permite que você gerencie atualizações e patches para vários computadores em um único local, em vez de em uma base por servidor. Com o gerenciamento de atualização do Azure, você pode rapidamente avaliar o status das atualizações disponíveis, agendar a instalação de atualizações necessárias e analisar os resultados de implantação para verificar as atualizações que se aplicam com êxito. Isso é possível se suas máquinas são VMs do Azure, hospedado por outros provedores de nuvem ou no local. [Saiba mais sobre o gerenciamento de atualização do Azure.](https://docs.microsoft.com/azure/automation/automation-update-management)

Com o Windows Admin Center, você pode facilmente configurar e usar o gerenciamento de atualização do Azure para manter seus servidores gerenciados atualizado. Se você ainda não tiver um espaço de trabalho do Log Analytics na sua assinatura do Azure, o Windows Admin Center automaticamente configurar seu servidor e criar os recursos necessários do Azure no local especificado e assinatura. Se você tiver um espaço de trabalho existente do Log Analytics, o Windows Admin Center pode configurar automaticamente o servidor para consumir atualizações de gerenciamento de atualização do Azure.  

Para começar, vá para a ferramenta de atualizações em uma conexão de servidor e selecione "Configurar agora" e forneça suas preferências para os recursos relacionados do Azure. 

Depois que você tiver configurado o servidor para ser gerenciado pelo gerenciamento de atualização do Azure, você pode acessar o gerenciamento de atualização do Azure usando o hiperlink fornecido na ferramenta de atualizações. 

[Aprenda a parar de usar o gerenciamento de atualização do Azure para atualizar seu servidor.](azure-monitor.md#disabling-monitoring)

Observe que você deve [registrar seu gateway do Windows Admin Center com o Azure](..\configure\azure-integration.md) antes de configurar o gerenciamento de atualização do Azure.

