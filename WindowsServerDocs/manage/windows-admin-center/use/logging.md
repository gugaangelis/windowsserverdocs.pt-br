---
title: Log de eventos
description: Log de eventos do Centro de administração do Windows (Project Paulo)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: d91b92cb3bba99ae4aa96a96650a251a6df4cea5
ms.sourcegitcommit: e0479b0114eac7f232e8b1e45eeede96ccd72b26
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2018
ms.locfileid: "2074337"
---
# <a name="use-event-logging-in-windows-admin-center-to-gain-insight-into-management-activities-and-track-gateway-usage"></a>Use o registro de eventos no Centro de administração do Windows para obter percepção atividades de gerenciamento e o uso de gateway da faixa

>Aplicável à: Centro de administração do Windows, a visualização do Centro de administração do Windows

Centro de administração do Windows grava os logs de eventos para que você veja as atividades de gerenciamento que está sendo executadas em servidores no seu ambiente, bem como para ajudá-lo a solucionar quaisquer problemas do Centro de administração do Windows.

## <a name="gain-insight-into-management-activities-in-your-environment-through-user-action-logging"></a>Percepção atividades de gerenciamento em seu ambiente por meio do log de ação do usuário

Centro de administração do Windows fornece ideias sobre as atividades de gerenciamento realizado em servidores no seu ambiente por ações de registro em log para o canal de evento **ServerManagementExperience da Microsoft** no log de eventos do servidor gerenciado, com EventID 4000 e SMEGateway de fonte. Centro de administração do Windows somente registra ações no servidor gerenciado, portanto você não verá os eventos registrados se um usuário acessa um servidor para fins de somente leitura.

Os eventos registrados incluem as seguintes informações:

| Chave           | Valor                                                                                              |
|---------------|----------------------------------------------------------------------------------------------------|
| PowerShell    | Nome do script do PowerShell que foi executado no servidor, se a ação tiver sido executado em um script do PowerShell |
| CIM           | Chamada CIM que foi executada no servidor, se a ação foi executada uma chamada CIM                        |
| Módulo        | Ferramenta (ou módulo) em que a ação foi executada                                                     |
| Gateway       | Nome da máquina de gateway do Centro de administração do Windows em que a ação foi executada                     |
| UserOnGateway | Nome de usuário usado para acessar o gateway do Centro de administração do Windows e executar a ação                    |
| UserOnTarget  | Nome de usuário usado para acessar o servidor de destino gerenciado, se for diferente do que o userOnGateway (ou seja, o usuário acessado usando o servidor usando credenciais "Gerenciar como") |
| Delegação    | Boolean: se o destino gerenciado servidor confia o gateway e delegação de credenciais de máquina do cliente do usuário             |
| LAPS          | Boolean: se o usuário acessada no servidor usando credenciais [LAPS](https://technet.microsoft.com/mt227395.aspx)                          |
| Arquivo          | nome do arquivo carregado, se a ação foi um carregamento de arquivo                                |

## <a name="learn-about-windows-admin-center-activity-with-event-logging"></a>Saiba mais sobre a atividade do Centro de administração do Windows com o log de eventos

Centro de administração do Windows registra as atividades de gateway para o canal de evento no computador do gateway para ajudá-lo a solucionar problemas e exibir métricas em uso. Esses eventos são registrados para o canal de evento **ServerManagementExperience da Microsoft** .

[Saiba mais sobre a solução de centro de administração do Windows.](troubleshooting.md)
