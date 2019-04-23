---
title: Recursos de compreensão
description: Este tópico define o conceito de recursos no sistema Insights e apresenta os recursos padrão disponíveis no Windows Server 2019.
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
ms.date: 6/05/2018
ms.openlocfilehash: 21932a3e45d7fc6711400eb30c63c3412bbc116e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830967"
---
# <a name="understanding-capabilities"></a>Recursos de compreensão

>Aplica-se a: Windows Server 2019

Este tópico define o conceito de recursos no sistema Insights e apresenta os recursos padrão disponíveis no Windows Server 2019. 

Este tópico também descreve as fontes de dados, linhas de previsão do tempo e status de previsão usadas para os recursos padrão. 

## <a name="capability-overview"></a>Visão geral da funcionalidade
Um recurso de Insights do sistema é um aprendizado de máquina ou modelo de estatística que analisa os dados do sistema para ajudar a lhe aumentado informações sobre o funcionamento da sua implantação. Informações do sistema apresenta um conjunto inicial de recursos padrão e permite que você adicione novos recursos dinamicamente, sem a necessidade de atualizar o sistema operacional. 

>[!NOTE]
>Documentação detalhada explicando como criar, adicionar e recursos de atualização está disponível [aqui](adding-and-developing-capabilities.md), e [o documento de recursos de gerenciamento](managing-capabilities.md) fornece informações mais alto nível sobre isso funcionalidade.

Além disso, cada recurso é executado localmente em uma instância do Windows Server, e cada recurso pode ser gerenciado individualmente.

### <a name="capability-outputs"></a>Saídas de recurso
Quando um recurso é invocado, ele fornece uma saída para ajudar a explicar o resultado da sua análise ou previsão. Cada saída deve conter um **Status** e uma **descrição do Status** descrever a previsão e cada resultado pode conter, opcionalmente, a dados de recurso específicas associados à previsão. O **descrição do Status** ajuda a fornecer uma explicação contextual para o **Status**, e a funcionalidade de relatórios um **Okey**, **aviso**, ou **crítico** status. Além disso, um recurso pode usar um **erro** ou **None** status se nenhuma previsão foi feita. Juntos, aqui estão os status de capacidade e seus significados básicos: 

- **Okey** -tudo parece certo.
- **Aviso** - nenhuma atenção imediata necessária, mas você deve dar uma olhada. 
- **Crítico** -você deve dar uma olhada em breve. 
- **Erro** -um problema desconhecido causou o recurso de failback. 
- **Nenhum** -nenhuma previsão foi feita. Isso pode ser devido à falta de dados ou qualquer outro motivo para não fazer uma previsão específica do recurso. 

Além disso, todos os dados específicos de recurso contidos no resultado serão colocados em um arquivo JSON acessível ao usuário e o caminho do arquivo [podem ser encontrados usando o PowerShell](https://docs.microsoft.com/windows-server/manage/system-insights/managing-capabilities#retrieving-capability-results). 

## <a name="default-capabilities"></a>Recursos padrão
No Windows Server de 2019, informações do sistema apresenta quatro recursos padrão voltados para previsão de capacidade:

- **Previsão de capacidade de CPU** -uso da CPU de previsões. 
- **Previsão de capacidade de rede** -previsões de uso para cada adaptador de rede de rede. 
- **Previsão de consumo de armazenamento total** -total de previsões de consumo de armazenamento em todas as unidades locais. 
- **Previsão de consumo de volume** -prevê o consumo de armazenamento para cada volume.

Cada funcionalidade analisa os dados históricos para prever o uso futuro, passado e **todos os recursos de previsão são projetados para prever tendências a longo prazo, em vez de comportamento de curto prazo**, ajudando os administradores corretamente provisionar hardware e ajustar suas cargas de trabalho para evitar a contenção de recursos no futuro. Como esses recursos se concentrar no uso de longo prazo, esses recursos analisar os dados diário. 

### <a name="forecasting-model"></a>Modelo de previsão
Os recursos padrão usam um modelo de previsão para prever o uso futuro e para cada previsão, o modelo é treinado localmente nos dados do seu computador. Esse modelo é projetado para ajudar a detectar tendências de prazo mais longas e treinamento em cada instância do Windows Server permite que a capacidade de adaptar-se para o comportamento específico e nuances do uso de cada máquina.

>[!NOTE]
>Determinando qual tipo de modelo a ser usado necessário testar vários modelos usando um conjunto de dados que contém dezenas de milhares de máquinas. Depois de analisar e ajustar esses modelos, decidimos usar um modelo de previsão de regressão automática, pois ela produz previsões altamente precisos e visualmente intuitivos, não exigindo muito tempo para treinar. Nesse modelo, no entanto, exige três semanas de dados de treinamento, portanto, cada recurso usa uma tendência linear básica até três semanas de dados disponíveis.

### <a name="forecasting-timelines"></a>As linhas do tempo de previsão
Os recursos padrão de previsão de um determinado número de dias no futuro com base no número de dias para o qual os dados foram coletados. A tabela a seguir mostra as linhas do tempo de previsão desses recursos:

| Tamanho de dados de entrada | Comprimento da previsão |
| --------------- | --------------- |
| 0-5 dias | Não há previsão é feita. |
| 6-180 dias | 1/3 * tamanho dos dados de entrada |
| 365 180 dias | 60 dias | 

### <a name="forecasting-data"></a>Dados de previsão
Cada funcionalidade analisa dados diários para fazer uma previsão uso futuro. CPU, rede e até mesmo uso do armazenamento, no entanto, pode frequentemente alterar ao longo do dia, ajustar dinamicamente às cargas de trabalho no computador. Como uso não é constante ao longo do dia, é importante representar corretamente o uso diário em um único ponto de dados. A tabela a seguir fornece detalhes sobre os pontos de dados específicos e como os dados são processados:


| Nome do recurso | Fontes de dados | Lógica de filtragem |
| --------------- | -------------- | ---------------- |
 Previsão de consumo de volume          | Tamanho do volume                    | Uso diário máximo              
 Previsão de consumo de armazenamento total   | Soma dos tamanhos de volume, a soma dos tamanhos de disco              | Uso diário máximo             
 Previsão de capacidade de CPU                | % Tempo do processador  | Média máxima de 2 horas por dia   
 Previsão de capacidade de rede         | Total de bytes/s         | Média máxima de 2 horas por dia  

Ao avaliar a lógica de filtragem acima, é importante observar que cada recurso de busca informar os administradores quando uso futuro será significativamente exceder a capacidade disponível – mesmo que 100% de utilização de CPU ocorrências momentaneamente, o uso da CPU pode não ter causado a contenção de recursos ou degradação significativa de desempenho. De CPU e rede, em seguida, deve haver prolongada de alto uso, em vez de picos momentâneo. Calcular a média da CPU e uso em todo o dia inteiro de rede, no entanto, pode perder informações de uso importante, como de algumas horas alta da CPU ou uso de rede significativamente pode afetar o desempenho de suas cargas de trabalho críticas. A média máxima de 2 horas durante cada dia evita esses extremos e ainda produz dados significativos para cada recurso de analisar.

Para o volume e o uso de armazenamento total, no entanto, a utilização de armazenamento não pode exceder a capacidade disponível, mesmo momentaneamente, portanto, o uso diário máximo é usado para esses recursos. 

### <a name="forecasting-statuses"></a>Status de previsão
Todos os recursos de sistema Insights devem saída um status associado com cada previsão. Cada funcionalidade padrão usa a lógica a seguir para definir o status de cada previsão:
- **OK**: A previsão não excede a capacidade disponível.
- **Aviso**: A previsão excede a capacidade disponível nos próximos 30 dias. 
- **Crítico**: A previsão excede a capacidade disponível nos próximos 7 dias. 
- **Erro**: O recurso teve um erro inesperado. 
- **Nenhum**: Não há dados suficientes para fazer uma previsão. Isso pode ser devido à falta de dados ou porque nenhum dado foi relatado recentemente.

>[!NOTE]
>Se a previsões de um recurso em várias instâncias - como vários volumes ou adaptadores de rede - o status reflete o status mais grave em todas as instâncias. Status individuais para cada adaptador de rede ou de volume são visíveis no Windows Admin Center ou nos dados contidos na saída de cada recurso. Para obter instruções sobre como analisar a saída JSON dos recursos padrão, visite [este blog](https://aka.ms/systeminsights-mitigationscripts). 


## <a name="see-also"></a>Consulte também
Para saber mais sobre o sistema Insights, use os seguintes recursos:

- [Visão geral de informações do sistema](overview.md)
- [Gerenciamento de recursos](managing-capabilities.md)
- [Adicionando e desenvolvimento de recursos](adding-and-developing-capabilities.md)
- [Perguntas Frequentes de Insights de sistema](faq.md)
