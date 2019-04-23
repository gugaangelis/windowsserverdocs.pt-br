---
title: Registro em log de eventos
description: Log de eventos do Windows Admin Center (projeto Paulo)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849297"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Usar eventos em log no Windows Admin Center para obter informações sobre as atividades de gerenciamento e acompanhar o uso de gateway

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

Windows Admin Center grava logs de eventos permitem que você veja as atividades de gerenciamento que está sendo executadas nos servidores em seu ambiente, bem como para ajudá-lo a solucionar problemas do Windows Admin Center.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Obtenha informações sobre as atividades de gerenciamento em seu ambiente com o log de ação do usuário

Windows Admin Center fornece informações sobre as atividades de gerenciamento executadas nos servidores em seu ambiente por ações de registro em log para o **ServerManagementExperience Microsoft** canal de evento no log de eventos de gerenciado servidor, com o EventID 4000 e SMEGateway do código-fonte. Windows Admin Center só registra ações no servidor gerenciado, portanto, você não verá os eventos registrados se um usuário acessa um servidor para propósitos de somente leitura.

Os eventos registrados incluem as seguintes informações:

| Chave           | Valor                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nome do script do PowerShell que foi executada no servidor, se a ação executou um script do PowerShell |
| CIM           | Chamada CIM que foi executada no servidor, se a ação executou uma chamada CIM                        |
| Módulo        | Ferramenta (ou módulo) em que a ação foi executada                                                     |
| Gateway       | Nome do computador do gateway do Windows Admin Center onde a ação foi executada                     |
| UserOnGateway | Nome de usuário usado para acessar o gateway do Windows Admin Center e executar a ação                    |
| UserOnTarget  | Nome de usuário usado para acessar o servidor de destino gerenciados, se for diferente de userOnGateway (ou seja, o usuário acessado usando o servidor usando credenciais "Gerenciar como") |
| Delegação    | Booliano: se o destino gerenciado confiança do servidor de gateway e delegação de credenciais de máquina do cliente do usuário             |
| LAPS          | Booliano: se o usuário acessou o servidor usando [LAPS](https://technet.microsoft.com/mt227395.aspx) credenciais                          |
| Arquivo          | nome do arquivo é carregado, se a ação foi um upload de arquivo                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>Saiba mais sobre a atividade do Windows Admin Center com o log de eventos

Windows Admin Center registra a atividade de gateway para o canal de evento no computador do gateway para ajudá-lo a solucionar problemas e exibir as métricas em uso. Esses eventos são registrados para o **ServerManagementExperience Microsoft** canal de evento.

[Saiba mais sobre como solucionar problemas do Windows Admin Center.](troubleshooting.md)
