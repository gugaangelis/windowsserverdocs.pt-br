---
title: Adicionando e recursos de desenvolvimento
description: O System insights permite que você adicione novos recursos de previsão ao System insights, sem a necessidade de qualquer atualização do sistema operacional. Isso permite que os desenvolvedores, incluindo a Microsoft e terceiros, criem e forneçam novos recursos de lançamento médio para atender aos cenários que você preocupa. Novos recursos podem especificar dados personalizados para coletar e analisar e também se integram aos planos de gerenciamento existentes do System insights.
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: b28621b24b321cdc22f07c03e9c04f0dde22759b
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87972013"
---
# <a name="adding-and-developing-new-capabilities"></a>Adicionando e desenvolvendo novos recursos

>Aplica-se a: Windows Server 2019

O System insights permite que você adicione novos recursos de previsão ao System insights, sem a necessidade de qualquer atualização do sistema operacional. Isso permite que os desenvolvedores, incluindo a Microsoft e terceiros, criem e forneçam novos recursos de lançamento médio para atender aos cenários que você preocupa.

Qualquer nova funcionalidade pode ser integrada com e estender a infraestrutura existente do insights do sistema:

- Novos recursos podem **especificar qualquer contador de desempenho ou evento do sistema**, que será coletado, persistido localmente e retornado para a capacidade de análise quando o recurso for invocado.
- Novos recursos podem **aproveitar os planos de gerenciamento do PowerShell e do centro de administração do Windows existentes**. Não apenas as novas funcionalidades serão detectáveis no System insights, elas também se beneficiarão de Agendamentos personalizados e ações de correção.

## <a name="manage-new-capabilities"></a>Gerenciar novos recursos
- [Saiba](add-remove-update-capabilities.md) como adicionar, remover e atualizar recursos usando o PowerShell.

## <a name="develop-a-capability"></a>Desenvolver um recurso
Use os recursos a seguir para ajudá-lo a começar a escrever seus próprios recursos personalizados:
- [Saiba mais](data-sources.md) sobre as fontes de dados que você pode coletar.
- [Baixe](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/) o pacote NuGet do System insights, que contém as classes e interfaces necessárias para escrever um recurso.
- [Visite](https://aka.ms/systeminsights-api) a documentação da API para saber mais sobre as classes e interfaces do System insights.
- [Use](https://aka.ms/systeminsights-samplecapability) o recurso de exemplo do System insights para ajudá-lo a começar. Isso mostra como registrar um recurso, especificar as fontes de dados a serem coletadas e começar a analisar os dados do sistema.

>[!NOTE]
>Essa é a funcionalidade de pré-lançamento. Ele está sujeito a alterações, à medida que adicionamos nova funcionalidade e incorporamos comentários.

## <a name="additional-references"></a>Referências adicionais
Para saber mais sobre o System insights, use os seguintes recursos:

- [Visão geral dos insights do sistema](overview.md)
- [Noções básicas dos recursos](understanding-capabilities.md)
- [Gerenciar recursos](managing-capabilities.md)
- [Adicionar, remover e atualizar recursos](add-remove-update-capabilities.md)
- [Perguntas frequentes do System insights](faq.md)