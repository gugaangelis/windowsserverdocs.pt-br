---
title: Perguntas Frequentes de Insights de sistema
description: Perguntas Frequentes de Insights de sistema
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: system-insights
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ''
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 5/23/2018
ms.openlocfilehash: 13767e1336d1ff729d1fbbe6cae3ed57d68cefc4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59851057"
---
# <a name="system-insights-faq"></a>Perguntas Frequentes de Insights de sistema

>Aplica-se a: Windows Server 2019

## <a name="how-can-you-use-system-insights-with-azure-monitor-or-system-center-operations-manager"></a>Como você pode usar informações do sistema com o Azure Monitor ou o System Center Operations Manager?

[O Azure Monitor](https://azure.microsoft.com/services/monitor/) e [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/welcome?view=sc-om-1807) fornecem informações operacionais em suas implantações para ajudá-lo a gerenciar sua infraestrutura. Insights de sistema, por outro lado, é um recurso do Windows Server que apresenta os recursos de análise preditiva local. Juntos, Insights de sistema e o Azure Monitor ou o SCOM pode ajudar as previsões de superfície em uma população de dispositivos:

 Azure Monitor ou SCOM pode chave desativar os eventos criados pelo sistema Insights, como o sistema Insights gera o resultado de cada previsão para o log de eventos. Eles podem surgir essas previsões específicas do computador em um grupo de servidores Windows, permitindo que você tenha uma exibição unificada dessas previsões em um grupo de instâncias de servidor. 
 
 Consulte as IDs de evento e de canal para cada previsão [aqui](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results).

## <a name="how-does-system-insights-relate-to-windows-ml"></a>Como o sistema de informações se relacionam com ML do Windows?

[Windows ML](https://docs.microsoft.com/windows/uwp/machine-learning/) é uma plataforma que permite aos desenvolvedores importar e pontuar modelos de aprendizado de máquina previamente treinado em dispositivos do Windows. Esses modelos se beneficiar de aceleração de hardware e podem ser pontuados localmente. 

Informações do sistema é um recurso no Windows Server 2019 que oferece funcionalidades preditivas locais junto com uma experiência completa de gerenciamento, incluindo a integração do PowerShell e Windows Admin Center. 

## <a name="can-i-use-system-insights-for-my-cluster"></a>Pode usar o sistema Insights para meu cluster? 

Sim. Insights de sistema podem executar independentemente em cada nó de cluster de failover individuais e o comportamento padrão de uso de previsões de Insights de sistema entre o armazenamento local, volume, CPU e rede. **Você também pode habilitar a previsão para o armazenamento em cluster**, portanto, os recursos de previsão de armazenamento prever o uso de volumes de cluster e armazenamento. 

Você pode gerenciar essas configurações no Windows Admin Center ou PowerShell, e informações mais detalhadas sobre essa funcionalidade estão disponíveis [aqui](https://blogs.technet.microsoft.com/filecab/2018/10/03/using-system-insights-to-forecast-clustered-storage-usage/).
 

## <a name="how-expensive-is-it-to-run-the-default-capabilities"></a>Quão caro é executar os recursos padrão?

Cada funcionalidade padrão é barata executar. Cada funcionalidade levará mais tempo para ser executado como coletar mais dados, mas eles normalmente devem ser concluídas em um apenas alguns segundos. 

## <a name="see-also"></a>Consulte também
Para saber mais sobre o sistema Insights, use os seguintes recursos:

- [Visão geral de informações do sistema](overview.md)
- [Recursos de compreensão](understanding-capabilities.md)
- [Gerenciamento de recursos](managing-capabilities.md)
- [Adicionando e desenvolvimento de recursos](adding-and-developing-capabilities.md)
