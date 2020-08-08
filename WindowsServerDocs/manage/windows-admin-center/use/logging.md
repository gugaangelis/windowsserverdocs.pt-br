---
title: Log de eventos
description: Log de eventos do centro de administração do Windows (projeto Honolulu)
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.openlocfilehash: addd9d4cf4516725ac8c59d84204cfeb2501e4b3
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87996565"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Usar o log de eventos no centro de administração do Windows para obter informações sobre as atividades de gerenciamento e acompanhar o uso do gateway

>Aplica-se a: Windows Admin Center, Versão prévia do Windows Admin Center

O centro de administração do Windows grava logs de eventos para permitir que você veja as atividades de gerenciamento executadas nos servidores do seu ambiente, bem como para ajudá-lo a solucionar problemas do centro de administração do Windows.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Aprofunde-se nas atividades de gerenciamento em seu ambiente por meio do registro em log de ações do usuário

O centro de administração do Windows fornece informações sobre as atividades de gerenciamento executadas nos servidores em seu ambiente, registrando ações no canal de eventos **Microsoft-ServerManagementExperience** no log de eventos do servidor gerenciado, com EventID 4000 e Source SMEGateway. O centro de administração do Windows apenas registra ações no servidor gerenciado, de modo que você não verá eventos registrados se um usuário acessar um servidor para fins somente leitura.

Os eventos registrados incluem as seguintes informações:

| Chave           | Valor                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | O nome do script do PowerShell que foi executado no servidor, se a ação tiver executado um script do PowerShell |
| CIM           | Chamada CIM que foi executada no servidor, se a ação executou uma chamada CIM                        |
| Módulo        | Ferramenta (ou módulo) em que a ação foi executada                                                     |
| Gateway       | Nome do computador do gateway do centro de administração do Windows em que a ação foi executada                     |
| UserOnGateway | Nome de usuário usado para acessar o gateway do centro de administração do Windows e executar a ação                    |
| UserOnTarget  | Nome de usuário usado para acessar o servidor gerenciado de destino, se for diferente do userOnGateway (ou seja, o usuário acessado usando o servidor usando as credenciais "gerenciar como") |
| Delegação    | Booliano: se o servidor gerenciado de destino confiar no gateway e as credenciais forem delegadas do computador cliente do usuário             |
| SOBREPOSTA          | Booliano: se o usuário acessasse o servidor usando credenciais [lapsas](/previous-versions/mt227395(v=msdn.10))                          |
| Arquivo          | nome do arquivo carregado, se a ação foi um upload de arquivo                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>Saiba mais sobre a atividade do centro de administração do Windows com o log de eventos

O centro de administração do Windows registra a atividade do gateway no canal de eventos no computador do gateway para ajudá-lo a solucionar problemas e exibir as métricas sobre o uso. Esses eventos são registrados no canal de eventos **Microsoft-ServerManagementExperience** .

[Saiba mais sobre como solucionar problemas do centro de administração do Windows.](../support/troubleshooting.md)