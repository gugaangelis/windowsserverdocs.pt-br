---
title: Ações de Serviço de Integridade
manager: eldenc
ms.author: cosdar
ms.topic: article
author: cosmosdarwin
ms.date: 08/14/2017
ms.openlocfilehash: 16e8a27dc38b8908ffb7ccac94f3bcc15a5c956f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87945789"
---
# <a name="health-service-actions"></a>Ações de Serviço de Integridade

> Aplica-se a: Windows Server 2019, Windows Server 2016

O Serviço de Integridade é um novo recurso do Windows Server 2016 que melhora o monitoramento diário e a experiência operacional para clusters que executam o Espaços de Armazenamento Diretos.

## <a name="actions"></a>Ações

A próxima seção descreve os fluxos de trabalho que são automatizados pelo Serviço de Integridade. Para verificar se uma ação está realmente sendo executada de forma autônoma ou para controlar seu andamento ou o resultado, o Serviço de Integridade gera "Ações". Diferentemente de logs, as ações desapareceram logo após serem concluídas e destinam-se principalmente a fornecer informações sobre a atividade em andamento, o que pode afetar o desempenho ou a capacidade (por exemplo, restaurar a resiliência ou rebalancear os dados).

### <a name="usage"></a>Uso

Um novo cmdlet do PowerShell exibe todas as ações:

```PowerShell
Get-StorageHealthAction
```

### <a name="coverage"></a>Cobertura

No Windows Server 2016, o cmdlet **Get-StorageHealthAction** pode retornar qualquer uma das seguintes informações:

-   Desativação com falha, perda de conectividade ou o disco físico não responde

-   Alternância de pool de armazenamento para usar o disco físico de substituição

-   Restauração de resiliência completa para dados

-   Rebalanceamento de pool de armazenamento

## <a name="additional-references"></a>Referências adicionais

- [Serviço de Integridade no Windows Server 2016](health-service-overview.md)
- [Documentação do desenvolvedor, código de exemplo e referência de API no MSDN](https://msdn.microsoft.com/windowshealthservice)
