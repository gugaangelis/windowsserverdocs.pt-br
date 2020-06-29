---
title: Fontes de dados de informações do sistema
description: Ao escrever um novo recurso no System insights, você pode especificar fontes de dados novas ou existentes para coletar localmente e analisar. Este tópico descreve as fontes de dados que você pode escolher ao registrar um novo recurso.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 7/31/2018
ms.openlocfilehash: 5dc44d9309c25ca1475e512a11d9868d7fa49e97
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85473663"
---
# <a name="system-insights-data-sources"></a>Fontes de dados de informações do sistema

>Aplica-se a: Windows Server 2019

As informações do sistema introduzem a funcionalidade de coleta de dados extensível. Ao escrever um novo recurso, você pode especificar fontes de dados novas ou existentes para coletar localmente e analisar. Este tópico descreve as fontes de dados que você pode escolher ao registrar um novo recurso.

## <a name="data-sources"></a>Fontes de dados
Ao escrever um novo recurso, você deve identificar as fontes de dados específicas a serem coletadas para cada recurso. As fontes de dados que você especificar serão coletadas e mantidas diretamente em seu computador, e você poderá escolher entre três tipos de fontes de dados:

- **Contadores de desempenho**:
    - Especifique o caminho, o nome e as instâncias do contador, e o System insights coleta os dados relevantes relatados por esses contadores de desempenho.

- **Eventos do sistema**:
    - Especifique o nome do canal e a ID do evento, e o System insights registrará quantas vezes esse evento ocorreu.

- **Série conhecida**
    - O System insights coleta algumas informações básicas em seu computador para alguns recursos bem definidos. Essas séries são usadas para os recursos padrão, mas também podem ser usadas por qualquer recurso personalizado. Eles coletam as seguintes informações:

        - **Disco**:
            - *Propriedades*: GUID
            - *Dados*: tamanho
        - **Volume**:
            - *Propriedades*: UniqueId, DriveLetter, FileSystemLabel, Size
            - *Dados*: tamanho usado
        - **Adaptador de rede**:
            - *Propriedades*: InterfaceGuid, InterfaceDescription, Speed
            - *Dados*: bytes recebidos/s, bytes enviados/s, total de bytes/s
        - **CPU**:
            - *Propriedades*:-
            - *Dados*:% de tempo do processador

    - Especifique uma série conhecida e as informações do sistema retornarão os dados coletados por essa série.


## <a name="retention-timelines-and-collection-intervals"></a>Linhas do tempo de retenção e intervalos de coleta
As fontes de dados acima têm linhas de tempo de retenção e intervalos de coleta diferentes. A tabela a seguir revela quanto tempo e com que frequência cada fonte de dados é coletada:

| Fonte de dados | Linha do tempo de retenção | Intervalo de coleta |
| --------------- | --------------- | ----------- |
| Contadores de desempenho | 3 meses | 15 minutos |
| Eventos do sistema | 3 meses | 15 minutos |
| Série conhecida | 1 ano | 1 hora |


### <a name="aggregation-types"></a>Tipos de agregação
Como cada série registra apenas um ponto de dados para cada intervalo de coleta, cada série tem um tipo de agregação associada-lo. A tabela a seguir descreve a fonte de dados e o tipo de agregação correspondente:

>[!NOTE]
>Para contadores de desempenho, você pode escolher entre alguns tipos diferentes de agregação.

| Fonte de dados | Tipos de agregação |
| --------------- | --------------- |
| Contadores de desempenho | Sum, Average, Max, min |
| Eventos do sistema | Contagem |
| Série bem conhecida de disco | Último (valor mais recente no intervalo de coleta) |
| Série bem conhecida de volume | Último (valor mais recente no intervalo de coleta) |
| Série bem conhecida de CPU | Média |
| Série bem conhecida de rede | Média |

## <a name="data-footprint"></a>Superfície de dados

O System insights coleta todos os dados localmente em sua unidade C (C:). Em geral, o volume de dados do System insights é modesto. Depende diretamente do tipo e do número de fontes de dados que cada funcionalidade especifica, e a tabela abaixo detalha o uso de armazenamento para cada tipo de dados:

| Fonte de dados | Superfície máxima |
| --------------- | --------------- |
| Contadores de desempenho | 240 KB |
| Eventos do sistema | 200 KB |
| Série bem conhecida de disco | 200 KB por disco |
| Série bem conhecida de volume | 300 KB por volume |
| Série bem conhecida de CPU | 100 KB |
| Série bem conhecida de rede | 300 KB por adaptador de rede |

>[!NOTE]
>**Para os recursos de previsão padrão, a superfície máxima deve ser inferior a 10 MB para a maioria das máquinas autônomas.**

## <a name="additional-references"></a>Referências adicionais
Para saber mais sobre o System insights, use os seguintes recursos:

- [Visão geral dos insights do sistema](overview.md)
- [Noções básicas dos recursos](understanding-capabilities.md)
- [Gerenciar recursos](managing-capabilities.md)
- [Adicionar e desenvolver recursos](adding-and-developing-capabilities.md)
- [Perguntas frequentes do System insights](faq.md)
