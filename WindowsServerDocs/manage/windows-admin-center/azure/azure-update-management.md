---
title: Use o Windows Admin Center para gerenciar atualizações de sistema operacional com o gerenciamento de atualizações do Azure
description: Use o Windows Admin Center (projeto Paulo) para configurar o gerenciamento de atualizações do Azure para gerenciar o sistema operacional de atualizações.
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.prod: windows-server-threshold
ms.openlocfilehash: 79b18e9963fba0993a7f34b1409edba6abfd48f0
ms.sourcegitcommit: 48bb3e5c179dc520fa879b16c9afe09e07c87629
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/31/2019
ms.locfileid: "66452545"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>Use o Windows Admin Center para gerenciar atualizações de sistema operacional com o gerenciamento de atualizações do Azure

[Saiba mais sobre a integração do Azure com Windows Admin Center.](../plan/azure-integration-options.md)

Gerenciamento de atualizações do Azure é uma solução de automação do Azure que permite que você gerencie atualizações e patches para várias máquinas de um único local, em vez de em uma base por servidor. Gerenciamento de atualizações do Azure, rapidamente avaliar o status de atualizações disponíveis, agendar a instalação de atualizações necessárias e examinar os resultados de implantação para verificar se as atualizações que foram aplicadas com sucesso. Isso é possível se suas máquinas são VMs do Azure, hospedado por outros provedores de nuvem ou no local. [Saiba mais sobre o gerenciamento de atualizações do Azure.](https://docs.microsoft.com/azure/automation/automation-update-management)

Com do Windows Admin Center, você pode facilmente configurar e usar o gerenciamento de atualizações do Azure para manter seus servidores gerenciados atualizados. Se você ainda não tiver um espaço de trabalho do Log Analytics em sua assinatura do Azure, o Windows Admin Center automaticamente configurar seu servidor e criar recursos do Azure necessários na assinatura e local que você especificar. Se você tiver um espaço de trabalho do Log Analytics existente, o Windows Admin Center pode configurar automaticamente seu servidor para consumir as atualizações do gerenciamento de atualização do Azure.  

Para começar, vá para a ferramenta de atualizações em uma conexão de servidor e selecione "Configurar agora" e fornecer suas preferências para os recursos relacionados do Azure. 

Depois que você configurou seu servidor para ser gerenciado pelo gerenciamento de atualizações do Azure, você pode acessar o gerenciamento de atualizações do Azure usando o hiperlink fornecido na ferramenta de atualizações. 

[Saiba como parar de usar o gerenciamento de atualizações do Azure para atualizar seu servidor.](azure-monitor.md#disabling-monitoring)

Observe que você deve [registrar seu gateway do Windows Admin Center com o Azure](../configure/azure-integration.md) antes de configurar o gerenciamento de atualizações do Azure.

