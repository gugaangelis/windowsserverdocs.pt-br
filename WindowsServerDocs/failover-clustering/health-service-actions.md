---
title: Ações de serviço de integridade
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: ''
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843017"
---
# <a name="health-service-actions"></a>Ações de serviço de integridade

> Aplica-se ao Windows Server 2016

O serviço de integridade é um novo recurso no Windows Server 2016, o que melhora o monitoramento de rotina e experiência operacional para clusters que executam espaços de armazenamento diretos.

## <a name="actions"></a>Ações  

A próxima seção descreve os fluxos de trabalho que são automatizados pelo Serviço de Integridade. Para verificar se uma ação está realmente sendo executada de forma autônoma ou para controlar seu andamento ou o resultado, o Serviço de Integridade gera "Ações". Diferentemente de logs, as ações desapareceram logo após serem concluídas e destinam-se principalmente a fornecer informações sobre a atividade em andamento, o que pode afetar o desempenho ou a capacidade (por exemplo, restaurar a resiliência ou rebalancear os dados).  

### <a name="usage"></a>Uso  

Um novo cmdlet do PowerShell exibe todas as ações:  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>Cobertura  

No Windows Server 2016, o **Get-StorageHealthAction** cmdlet pode retornar qualquer uma das seguintes informações:  

-   Desativação com falha, perda de conectividade ou o disco físico não responde  

-   Alternância de pool de armazenamento para usar o disco físico de substituição  

-   Restauração de resiliência completa para dados  

-   Rebalanceamento de pool de armazenamento  

## <a name="see-also"></a>Consulte também

- [Serviço de integridade no Windows Server 2016](health-service-overview.md)
- [Documentação do desenvolvedor, código de exemplo e referência de API no MSDN](https://msdn.microsoft.com/windowshealthservice)
