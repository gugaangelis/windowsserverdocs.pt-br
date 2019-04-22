---
title: Adicionando e recursos de desenvolvimento
description: Informações do sistema permite que você adicionar novos recursos de previsão para o sistema Insights, sem a necessidade de quaisquer atualizações do sistema operacional. Isso permite que desenvolvedores, incluindo a Microsoft e terceiros, para criar e fornecer a nova versão intermediária de recursos para lidar com os cenários que importantes para você. Novos recursos podem especificar dados personalizados para coletar e analisar, e eles também integram com planos existentes de gerenciamento de informações do sistema.
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
ms.openlocfilehash: 8caddead774ac69a38906f3c0a0d2eaf005c1d28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59817477"
---
# <a name="adding-and-developing-new-capabilities"></a>Adicionando e desenvolver novos recursos

>Aplica-se a: Windows Server 2019

Informações do sistema permite que você adicionar novos recursos de previsão para o sistema Insights, sem a necessidade de quaisquer atualizações do sistema operacional. Isso permite que desenvolvedores, incluindo a Microsoft e terceiros, para criar e fornecer a nova versão intermediária de recursos para lidar com os cenários que importantes para você. 

Um novo recurso pode se integrar e estender a infraestrutura existente do sistema de informações:

- Novos recursos podem **especificar qualquer evento de sistema ou contador de desempenho**, que será coletado, mantido localmente e retornado para o recurso para análise quando o recurso é invocado.  
- Novos recursos podem **aproveitar os existentes de Windows Admin Center e planos de gerenciamento do PowerShell**. Não só será novos recursos podem ser descobertos no sistema Insights, eles também se beneficiar de agendas personalizadas e ações de correção. 

## <a name="manage-new-capabilities"></a>Gerenciar novos recursos
- [Saiba mais](add-remove-update-capabilities.md) como adicionar, remover e atualizar recursos usando o PowerShell. 

## <a name="develop-a-capability"></a>Desenvolver um recurso
Use os seguintes recursos para ajudá-lo a começar a escrever seus próprios recursos personalizados:
- [Saiba mais](data-sources.md) sobre as fontes de dados que você pode coletar.
- [Baixar](https://www.nuget.org/packages/Microsoft.WindowsServer.SystemInsights/) o pacote NuGet de Insights do sistema, que contém as classes e interfaces que você precisa escrever uma funcionalidade.
- [Visite](https://aka.ms/systeminsights-api) a documentação da API para saber mais sobre as interfaces e classes de informações do sistema. 
- [Use](https://aka.ms/systeminsights-samplecapability) a capacidade de amostra de Insights de sistema para ajudar você a começar. Isso mostra como registrar um recurso, especifique as fontes de dados para coletar e começar a analisar dados do sistema.

>[!NOTE]
>Essa é uma funcionalidade de pré-lançamento. Ela está sujeita a alterações, como podemos adicionar uma nova funcionalidade e incorporar o feedback.

## <a name="see-also"></a>Consulte também
Para saber mais sobre o sistema Insights, use os seguintes recursos:

- [Visão geral de informações do sistema](overview.md)
- [Recursos de compreensão](understanding-capabilities.md)
- [Gerenciamento de recursos](managing-capabilities.md)
- [Adicionando, removendo e atualizando recursos](add-remove-update-capabilities.md)
- [Perguntas Frequentes de Insights de sistema](faq.md)