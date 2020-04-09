---
title: Adicionar, remover e atualizar recursos
description: O System insights permite que você crie novos recursos que aproveitam a funcionalidade de gerenciamento e coleta de dados existentes. É importante que você também tenha o suporte à plataforma para gerenciar a adição, remoção e atualizações desses recursos. Este tópico descreve a funcionalidade de alto nível para adicionar, remover e atualizar recursos no System insights.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 0a760f25de79bc89b2aa67aec6bb1e3a493c1310
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80856259"
---
# <a name="adding-removing-and-updating-capabilities"></a>Adicionar, remover e atualizar recursos

>Aplica-se a: Windows Server 2019

O System insights permite que você crie novos recursos que aproveitam a funcionalidade de gerenciamento e coleta de dados existentes. Quando esses recursos são criados, no entanto, é igualmente importante que você também tenha o suporte da plataforma para gerenciar a adição, remoção e atualizações desses recursos. 

Este tópico descreve a funcionalidade de alto nível para adicionar, remover e atualizar recursos no System insights. 

## <a name="adding-a-capability"></a>Adicionando um recurso
O System insights permite que você adicione novos recursos a qualquer momento usando o cmdlet **Add-InsightsCapability** . O **Add-InsightsCapability** exige que você especifique um nome de recurso e a biblioteca de recursos. A biblioteca de recursos contém a descrição da funcionalidade, as fontes de dados e a lógica de previsão.

```PowerShell
Add-InsightsCapability -Name Sample capability -Library C:\SampleCapability.dll
```

Depois que um recurso tiver sido adicionado ao insights do sistema, você poderá invocar e gerenciar imediatamente o recurso usando o PowerShell ou o centro de administração do Windows. 

## <a name="updating-a-capability"></a>Atualizando um recurso
O System insights também permite que você atualize um recurso usando o cmdlet **Update-InsightsCapability** .

```PowerShell
Update-InsightsCapability -Name Sample capability -Library C:\SampleCapabilityv2.dll
```

A atualização de um recurso permite que você especifique uma nova biblioteca de recursos, que permite alterar a descrição da capacidade, as fontes de dados e a lógica de previsão associada a esse recurso. De forma importante, a atualização de uma funcionalidade preserva todas as informações de configuração e históricas sobre esse recurso, incluindo agendas personalizadas, ações e resultados de previsão históricas. 

## <a name="removing-a-capability"></a>Removendo um recurso
Você também pode remover funcionalidades do System insights usando o cmdlet **Remove-InsightsCapability** . 

```PowerShell
Remove-InsightsCapability -Name Sample capability 
```
>[!NOTE]
>Os recursos de previsão padrão não podem ser removidos.

Remover um recurso exclui permanentemente o recurso e todas as informações associadas, incluindo o agendamento, as ações de correção e os resultados de previsão anteriores. 

>[!TIP]
>Considere desabilitar uma funcionalidade em vez de removê-la se você estiver preocupado com a exclusão permanente de todas as informações associadas à funcionalidade. 

## <a name="see-also"></a>Consulte também
Para saber mais sobre o System insights, use os seguintes recursos:

- [Visão geral do System insights](overview.md)
- [Noções básicas dos recursos](understanding-capabilities.md)
- [Gerenciar recursos](managing-capabilities.md)
- [Adicionar e desenvolver recursos](adding-and-developing-capabilities.md)
- [Perguntas frequentes do System insights](faq.md)