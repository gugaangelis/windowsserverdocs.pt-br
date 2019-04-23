---
title: Fontes de dados de informações do sistema
description: Ao escrever uma nova funcionalidade no sistema Insights, você pode especificar as fontes de dados novo ou existente para coletar localmente e analisar. Este tópico descreve as fontes de dados que você pode escolher ao registrar uma nova funcionalidade.
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
ms.openlocfilehash: 9b46db90787d24a173ffa472ec1ecb8eaffe054b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845407"
---
# <a name="system-insights-data-sources"></a>Fontes de dados de informações do sistema

>Aplica-se a: Windows Server 2019

Informações do sistema apresenta a funcionalidade de coleta de dados extensível. Ao escrever uma nova funcionalidade, você pode especificar as fontes de dados novo ou existente para coletar localmente e analisar. Este tópico descreve as fontes de dados que você pode escolher ao registrar uma nova funcionalidade.

## <a name="data-sources"></a>Fontes de dados
Ao escrever uma nova funcionalidade, você deve identificar as fontes de dados específico a ser obtida para cada recurso. As fontes de dados que você especificar serão coletadas e mantidas diretamente em seu computador, e você pode escolher entre três tipos de fontes de dados:

- **Contadores de desempenho**: 
    - Especifique o caminho do contador, o nome e a instâncias e sistema Insights coleta os dados relevantes relatados por esses contadores de desempenho. 

- **Eventos do sistema**:
    - Especifique a ID de evento e o nome do canal e Insights de sistema registrará quantas vezes que o evento ocorreu.

- **Série Well-Known**
    - Sistema Insights coleta algumas informações básicas em seu computador para um bem definido, alguns recursos. Essas séries são usadas para os recursos padrão, mas eles também podem ser usados por qualquer recurso personalizado. Eles coletam as seguintes informações:

        - **Disco**: 
            - *Propriedades*: Guid
            - *Dados*: Tamanho
        - **Volume**:
            - *Propriedades*: UniqueId, DriveLetter, FileSystemLabel, Size
            - *Dados*: Tamanho usado
        - **Adaptador de rede**:
            - *Propriedades*: InterfaceGuid, InterfaceDescription, Speed
            - *Dados*: Bytes recebidos/s, Bytes enviados/s, Total de Bytes/s
        - **CPU**: 
            - *Propriedades*:-
            - *Dados*: % tempo do processador

    - Especificam uma série bem conhecida e Insights de sistema retorna os dados coletados por essa série. 


## <a name="retention-timelines-and-collection-intervals"></a>Intervalos de coleta e linhas do tempo de retenção
As fontes de dados acima têm intervalos de coleta e linhas do tempo de retenção diferente. A tabela a seguir revela como longo e com que frequência cada fonte de dados é coletado:

| Fonte de dados | Linha do tempo de retenção | Intervalo de coleta |
| --------------- | --------------- | ----------- |
| Contadores de desempenho | 3 meses | 15 minutos |
| Eventos do sistema | 3 meses | 15 minutos |
| Série Well-Known | 1 ano | 1 hora |


### <a name="aggregation-types"></a>Tipos de agregação
Como cada série registram apenas um ponto de dados para cada intervalo de coleta, cada série tem uma conta do tipo de agregação-lo. A tabela a seguir descreve a fonte de dados e o tipo de agregação correspondente:

>[!NOTE]
>Para contadores de desempenho, você pode escolher entre alguns tipos de agregação diferente.

| Fonte de dados | Tipos de agregação |
| --------------- | --------------- |
| Contadores de desempenho | Sum, max, min a média, |
| Eventos do sistema | Contagem |
| Série de disco bem conhecido | Último (valor mais recente no intervalo de coleta) |
| Série bem conhecido de volume | Último (valor mais recente no intervalo de coleta) |
| Série bem conhecido de CPU | Média |
| Série bem conhecido de rede | Média |

## <a name="data-footprint"></a>Volume de dados

Sistema Insights coleta todos os dados localmente em sua unidade C (c). Em geral, o volume de dados do sistema Insights é pequeno. Ele depende diretamente o tipo e o número de fontes de dados que especifica cada funcionalidade e a tabela a seguir fornece detalhes sobre o uso do armazenamento para cada tipo de dados:

| Fonte de dados | Espaço máximo |
| --------------- | --------------- |
| Contadores de desempenho | 240 KB |
| Eventos do sistema | 200 KB |
| Série de disco bem conhecido | 200 KB por disco |
| Série bem conhecido de volume | 300 KB por volume |
| Série bem conhecido de CPU | 100 KB |
| Série bem conhecido de rede | 300 KB por adaptador de rede |

>[!NOTE]
>**Os recursos de previsão por padrão, o volume máximo deve ser menor que 10 MB para a maioria dos computadores de maneira autônoma.** 

## <a name="see-also"></a>Consulte também
Para saber mais sobre o sistema Insights, use os seguintes recursos:

- [Visão geral de informações do sistema](overview.md)
- [Recursos de compreensão](understanding-capabilities.md)
- [Gerenciamento de recursos](managing-capabilities.md)
- [Adicionando e desenvolvimento de recursos](adding-and-developing-capabilities.md)
- [Perguntas Frequentes de Insights de sistema](faq.md)
