---
title: Adicionar, remover e atualizar recursos
description: Informações do sistema permite que você crie novos recursos que aproveitam a coleta de dados existente e a funcionalidade de gerenciamento. É importante que você também tem o suporte da plataforma para gerenciar a adição, remoção e as atualizações desses recursos. Este tópico descreve a funcionalidade de alto nível para adicionar, remover e atualizar recursos do sistema de informações.
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
ms.date: 7/31/2018
ms.openlocfilehash: 07fb036d1c4aa4a63107594ec1f81cb5be1c7724
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880157"
---
# <a name="adding-removing-and-updating-capabilities"></a>Adicionar, remover e atualizar recursos

>Aplica-se a: Windows Server 2019

Informações do sistema permite que você crie novos recursos que aproveitam a coleta de dados existente e a funcionalidade de gerenciamento. Depois que esses recursos são criados, no entanto, é igualmente importante que você também tem o suporte da plataforma para gerenciar a adição, remoção e as atualizações desses recursos. 

Este tópico descreve a funcionalidade de alto nível para adicionar, remover e atualizar recursos do sistema de informações. 

## <a name="adding-a-capability"></a>Adicionar um recurso
Informações do sistema permite que você adicione novos recursos a qualquer momento usando o **InsightsCapability adicionar** cmdlet. O **InsightsCapability adicionar** exige que você especifique um nome de recurso e a biblioteca de funcionalidade. A biblioteca de recurso contém a descrição do recurso, fontes de dados e lógica de previsão.

```PowerShell
Add-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapability.dll"
```

Depois que um recurso foi adicionado ao sistema Insights, você pode invocar e gerenciar o recurso usando o PowerShell ou Windows Admin Center imediatamente. 

## <a name="updating-a-capability"></a>Atualizando um recurso
Informações do sistema também permite que você atualize um recurso usando o **InsightsCapability atualização** cmdlet.

```PowerShell
Update-InsightsCapability -Name "Sample capability" -Library "C:\SampleCapabilityv2.dll"
```

Atualizar um recurso permite que você especifique uma nova biblioteca de funcionalidade, que permite que você altere a descrição do recurso, as fontes de dados e a lógica de previsão associada com esse recurso. É importante, atualizar um recurso preserva todas as configuração e informações históricas sobre esse recurso, incluindo agendas personalizadas, ações e resultados de previsão histórica. 

## <a name="removing-a-capability"></a>Remover um recurso
Você também pode remover recursos de sistema Insights usando o **InsightsCapability remover** cmdlet. 

```PowerShell
Remove-InsightsCapability -Name "Sample capability" 
```
>[!NOTE]
>O padrão de recursos de previsão não pode ser removido.

Removendo uma funcionalidade permanentemente exclui o recurso e todos os respectivos informações, incluindo o agendamento, as ações de correção e últimos resultados da previsão. 

>[!TIP]
>Considere desabilitar um recurso em vez de removê-lo se você estiver preocupado com a exclusão permanente de todas as informações associadas com a capacidade. 

## <a name="see-also"></a>Consulte também
Para saber mais sobre o sistema Insights, use os seguintes recursos:

- [Visão geral de informações do sistema](overview.md)
- [Recursos de compreensão](understanding-capabilities.md)
- [Gerenciamento de recursos](managing-capabilities.md)
- [Adicionando e desenvolvimento de recursos](adding-and-developing-capabilities.md)
- [Perguntas Frequentes de Insights de sistema](faq.md)