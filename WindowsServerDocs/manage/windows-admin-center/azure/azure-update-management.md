---
title: Usar o centro de administração do Windows para gerenciar atualizações do sistema operacional com o Azure Gerenciamento de Atualizações
description: Use o centro de administração do Windows (projeto Honolulu) para configurar o Azure Gerenciamento de Atualizações para gerenciar atualizações do sistema operacional.
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 07/17/2018
ms.localizationpriority: low
ms.openlocfilehash: 3818e06780ac22f56ed3d44209041f58c1070409
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87940057"
---
# <a name="use-windows-admin-center-to-manage-operating-system-updates-with-azure-update-management"></a>Usar o centro de administração do Windows para gerenciar atualizações do sistema operacional com o Azure Gerenciamento de Atualizações

[Saiba mais sobre a integração do Azure com o Windows Admin Center.](../plan/azure-integration-options.md)

O Azure Gerenciamento de Atualizações é uma solução na automação do Azure que permite que você gerencie atualizações e patches para vários computadores de um único local, em vez de por servidor. Com o Gerenciamento de Atualizações do Azure, é possível avaliar rapidamente o status de atualizações disponíveis, agendar a instalação de atualizações necessárias e examinar os resultados de implantação para verificar atualizações que foram aplicadas com êxito. Isso é possível se seus computadores forem VMs do Azure, hospedados por outros provedores de nuvem ou no local. [Saiba mais sobre o Azure Gerenciamento de Atualizações.](https://docs.microsoft.com/azure/automation/automation-update-management)

Com o centro de administração do Windows, você pode facilmente configurar e usar o Gerenciamento de Atualizações do Azure para manter seus servidores gerenciados atualizados. Se você ainda não tiver um espaço de trabalho Log Analytics em sua assinatura do Azure, o centro de administração do Windows configurará automaticamente seu servidor e criará os recursos do Azure necessários na assinatura e no local que você especificar. Se você tiver um espaço de trabalho Log Analytics existente, o centro de administração do Windows poderá configurar automaticamente seu servidor para consumir atualizações do Gerenciamento de Atualizações do Azure.

Para começar, vá para a ferramenta atualizações em uma conexão de servidor e selecione "configurar agora" e forneça suas preferências para os recursos do Azure relacionados.

Depois de configurar o servidor para ser gerenciado pelo Azure Gerenciamento de Atualizações, você poderá acessar o Azure Gerenciamento de Atualizações usando o hiperlink fornecido na ferramenta de atualizações.

[Saiba como parar de usar o Azure Gerenciamento de Atualizações para atualizar o servidor.](azure-monitor.md#disabling-monitoring)

Observe que você deve [registrar o gateway do centro de administração do Windows com o Azure](../configure/azure-integration.md) antes de configurar o Azure gerenciamento de atualizações.

