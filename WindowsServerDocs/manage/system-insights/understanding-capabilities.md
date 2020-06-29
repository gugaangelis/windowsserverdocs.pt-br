---
title: Recursos de compreensão
description: Este tópico define o conceito de recursos no System insights e apresenta os recursos padrão disponíveis no Windows Server 2019.
ms.prod: windows-server
ms.technology: system-insights
ms.topic: article
author: gawatu
ms.author: gawatu
manager: mallikarjun.chadalapaka
ms.date: 6/05/2018
ms.openlocfilehash: c6738e6e914d97c70aa31af2fe3b6987b0b9ea33
ms.sourcegitcommit: 771db070a3a924c8265944e21bf9bd85350dd93c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85471741"
---
# <a name="understanding-capabilities"></a>Recursos de compreensão

>Aplica-se a: Windows Server 2019

Este tópico define o conceito de recursos no System insights e apresenta os recursos padrão disponíveis no Windows Server 2019.

Este tópico também descreve as fontes de dados, as linhas do tempo de previsão e os status de previsão usados para os recursos padrão.

## <a name="capability-overview"></a>Visão geral da funcionalidade
Um recurso de informações do sistema é um modelo de aprendizado de máquina ou estatísticas que analisa os dados do sistema para ajudar a fornecer uma visão mais detalhada do funcionamento da sua implantação. O System insights apresenta um conjunto inicial de recursos padrão e permite que você adicione novos recursos dinamicamente, sem a necessidade de atualizar o sistema operacional.

>[!NOTE]
>A documentação detalhada que explica como criar, adicionar e atualizar recursos está disponível [aqui](adding-and-developing-capabilities.md), e [o documento gerenciamento de recursos](managing-capabilities.md) fornece informações mais detalhadas sobre essa funcionalidade.

Além disso, cada funcionalidade é executada localmente em uma instância do Windows Server, e cada recurso pode ser gerenciado individualmente.

### <a name="capability-outputs"></a>Saídas de funcionalidade
Quando um recurso é invocado, ele fornece uma saída para ajudar a explicar o resultado de sua análise ou previsão. Cada saída deve conter um **status** e uma **Descrição de status** para descrever a previsão, e cada resultado pode, opcionalmente, conter dados específicos de recursos associados à previsão. A **Descrição do status** ajuda a fornecer uma explicação contextual para o **status**, e a funcionalidade relata um status **OK**, **aviso**ou **crítico** . Além disso, um recurso pode usar um **erro** ou **nenhum** status se nenhuma previsão tiver sido feita. Juntos, aqui estão os status de capacidade e seus significados básicos:

- **OK** -tudo parece bom.
- **Aviso** -nenhuma atenção imediata é necessária, mas você deve dar uma olhada.
- **Crítico** -você deve dar uma olhada em breve.
- **Erro** -um problema desconhecido causou a falha na capacidade.
- **Nenhum** -nenhuma previsão foi feita. Isso pode ser devido à falta de dados ou a qualquer outro motivo específico de recurso para não fazer uma previsão.

Além disso, todos os dados específicos de recurso contidos no resultado serão colocados em um arquivo JSON acessível pelo usuário e o caminho do arquivo [poderá ser encontrado usando o PowerShell](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results).

## <a name="default-capabilities"></a>Recursos padrão
No Windows Server 2019, o System insights apresenta quatro recursos padrão com foco na previsão de capacidade:

- **Previsão de capacidade de CPU** – prevê o uso da CPU.
- **Previsão de capacidade de rede** – prevê o uso da rede para cada adaptador de rede.
- **Previsão de consumo de armazenamento total** – prevê o consumo de armazenamento total em todas as unidades locais.
- **Previsão de consumo de volume** – prevê o consumo de armazenamento para cada volume.

Cada funcionalidade analisa os dados históricos anteriores para prever o uso futuro, e **todos os recursos de previsão foram projetados para prever tendências de longo prazo em vez do comportamento de curto prazo**, ajudando os administradores a provisionar corretamente o hardware e ajustar suas cargas de trabalho para evitar a contenção de recursos futuramente. Como esses recursos se concentram no uso de longo prazo, esses recursos analisam dados diários.

### <a name="forecasting-model"></a>Modelo de previsão
Os recursos padrão usam um modelo de previsão para prever o uso futuro e, para cada previsão, o modelo é treinado localmente nos dados do seu computador. Esse modelo foi projetado para ajudar a detectar tendências de prazo mais longo, e o novo treinamento em cada instância do Windows Server permite que a capacidade se adapte ao comportamento específico e às nuances do uso de cada computador.

>[!NOTE]
>Determinar o tipo de modelo a ser usado para testar muitos modelos usando um conjunto de uma série de milhares de computadores. Depois de analisar e ajustar esses modelos, decidimos usar um modelo de previsão de regressão automática, pois ele produz previsões altamente precisas e intuitivas, sem exigir muito tempo para treinar. No entanto, esse modelo requer três semanas de dados de treinamento; portanto, cada recurso usa uma tendência linear básica até que três semanas de dados estejam disponíveis.

### <a name="forecasting-timelines"></a>Previsão de linha do tempo
Os recursos padrão prevêem um determinado número de dias no futuro com base no número de dias para os quais os dados foram coletados. A tabela a seguir mostra as linhas do tempo de previsão desses recursos:

| Tamanho dos dados de entrada | Comprimento da previsão |
| --------------- | --------------- |
| 0-5 dias | Nenhuma previsão é feita. |
| 6-180 dias | 1/3 * tamanho dos dados de entrada |
| 180-365 dias | 60 dias |

### <a name="forecasting-data"></a>Dados de previsão
Cada funcionalidade analisa os dados diários para prever o uso futuro. A CPU, a rede e, até mesmo, o uso do armazenamento, no entanto, pode mudar com freqüência ao longo do dia, ajustando-se dinamicamente às cargas de trabalho no computador. Como o uso não é constante ao longo do dia, é importante representar corretamente o uso diário em um único ponto de dados. A tabela abaixo detalha os pontos de dados específicos e como os dados são processados:


| Nome do recurso | Fonte (s) de dados | Lógica de filtragem |
| --------------- | -------------- | ---------------- |
 Previsão de consumo de volume          | Tamanho do volume                    | Uso diário máximo
 Previsão de consumo de armazenamento total   | Soma dos tamanhos de volume, soma dos tamanhos de disco              | Uso diário máximo
 Previsão de capacidade de CPU                | % Tempo do Processador  | Média máxima de 2 horas por dia
 Previsão de capacidade de rede         | Bytes Totais/s         | Média máxima de 2 horas por dia

Ao avaliar a lógica de filtragem acima, é importante observar que cada recurso procura informar os administradores quando o uso futuro excederá significativamente a capacidade disponível – mesmo que a CPU pressione momentaneamente 100% de utilização, o uso da CPU pode não ter causado uma degradação significativa de desempenho ou contenção de recursos. Para CPU e rede, deve haver um alto uso sustentado em vez de picos momentâneos. A média do uso da CPU e da rede em todo o dia, no entanto, perderia informações de uso importantes, pois algumas horas de alto uso da CPU ou da rede poderiam impactar significativamente o desempenho de suas cargas de trabalho críticas. A média máxima de 2 horas durante cada dia evita esses extremos e ainda produz dados significativos para cada recurso a ser analisado.

No entanto, para o uso de volume e armazenamento total, a utilização de armazenamento não pode exceder a capacidade disponível, mesmo momentaneamente, portanto, o uso diário máximo é usado para esses recursos.

### <a name="forecasting-statuses"></a>Status de previsão
Todos os recursos de informações do sistema devem gerar um status associado a cada previsão. Cada funcionalidade padrão usa a seguinte lógica para definir cada status de previsão:
- **OK**: a previsão não excede a capacidade disponível.
- **Aviso**: a previsão excede a capacidade disponível nos próximos 30 dias.
- **Crítico**: a previsão excede a capacidade disponível nos próximos 7 dias.
- **Erro**: a funcionalidade encontrou um erro inesperado.
- **Nenhum**: não há dados suficientes para fazer uma previsão. Isso pode ser devido à falta de dados ou porque nenhum dado foi relatado recentemente.

>[!NOTE]
>Se um recurso for previsões em várias instâncias, como vários volumes ou adaptadores de rede, o status refletirá o status mais grave em todas as instâncias. Os status individuais de cada volume ou adaptador de rede são visíveis no centro de administração do Windows ou dentro dos dados contidos na saída de cada recurso. Para obter instruções sobre como analisar a saída JSON dos recursos padrão, visite [este blog](https://aka.ms/systeminsights-mitigationscripts).


## <a name="additional-references"></a>Referências adicionais
Para saber mais sobre o System insights, use os seguintes recursos:

- [Visão geral dos insights do sistema](overview.md)
- [Gerenciar recursos](managing-capabilities.md)
- [Adicionar e desenvolver recursos](adding-and-developing-capabilities.md)
- [Perguntas frequentes do System insights](faq.md)
