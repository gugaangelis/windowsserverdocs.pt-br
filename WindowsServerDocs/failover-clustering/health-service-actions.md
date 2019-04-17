---
title: "Ações de serviço de integridade"
ms.prod: windows-server-threshold
manager: eldenc
ms.author: cosdar
ms.technology: storage-health-service
ms.topic: article
ms.assetid: 
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: efdf8f04e68fcbdc7051e78d6725cb919e740ffa
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2017
---
# <a name="health-service-actions"></a>Ações de serviço de integridade

> Aplica-se para o Windows Server 2016

O serviço de integridade é um novo recurso no Windows Server 2016 que melhora o monitoramento diárias e experiência operacional para clusters que executam direta de espaços de armazenamento.

## <a name="actions"></a>Ações  

A próxima seção descreve os fluxos de trabalho que sejam automatizados pelo serviço de integridade. Para verificar se uma ação é realmente está sendo executada autonomia, ou para acompanhar seu progresso ou resultado, o serviço de integridade gera "Ações". Ao contrário de logs, ações desaparecem logo depois que tiverem terminado e destinam-se principalmente a fornecer a percepção de atividade em andamento que pode afetar o desempenho ou capacidade (por exemplo, restaurando resiliência ou redistribuição dados).  

### <a name="usage"></a>Uso  

Um novo cmdlet do PowerShell exibe todas as ações:  

```PowerShell
Get-StorageHealthAction  
```

### <a name="coverage"></a>Cobertura  

No Windows Server 2016, o **Get-StorageHealthAction** cmdlet pode retornar qualquer uma das seguintes informações:  

-   Desativação de conectividade com falha, perdida ou sem resposta disco físico  

-   Alternar o pool de armazenamento para usar o disco físico de substituição  

-   Restaurando resiliência completa aos dados  

-   Redistribuição pool de armazenamento  

## <a name="see-also"></a>Consulte também

- [Serviço de integridade no Windows Server 2016](health-service-overview.md)
- [Documentação do desenvolvedor, exemplo de código e referência de API no MSDN](https://msdn.microsoft.com/windowshealthservice)
