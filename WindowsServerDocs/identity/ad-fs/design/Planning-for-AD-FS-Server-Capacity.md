---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: Planejando para o AD FS servidor capacidade
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 484dd08edef85b91e777f8963f175a6172c75430
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="planning-for-ad-fs-server-capacity"></a>Planejando para o AD FS servidor capacidade

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

  
> [!NOTE]  
> O conteúdo fornecido neste tópico não reflete o teste real que foi realizada em servidores que executam o Windows Server 2012. Este tópico será atualizado depois de realizar os testes necessários.  
  
Capacidade de planejamento de serviços de Federação do Active Directory \(AD FS\) é o processo de planejamento e previsão de pico de uso de seu serviço de Federação ou scaling\-up a implantação do servidor do AD FS para atender aos requisitos de carregar.  
  
Esta seção descreve as diretrizes de implantação para o servidor de Federação e funções de proxy do servidor de Federação e baseia-se em testes de laboratório que foi realizada pela equipe de produto do AD FS na Microsoft. O objetivo deste conteúdo é para ajudá-lo:  
  
-   Estime estreitamente às necessidades de hardware para a implantação do AD FS específica da sua organização, como o número de servidores do AD FS.  
  
-   Com precisão projetar o pico esperado para sign\ em solicitações, planejar o crescimento e garantir que a implantação do AD FS é capaz de manipulação que esperado pico de uso.  
  
Antes de prosseguir com esse conteúdo de planejamento da capacidade de leitura, recomendamos concluir primeiro as tarefas na ordem mostrada nas duas tabelas a seguir. A primeira tabela, fornecemos links para tarefas recomendadas que ajudarão a fornecem contexto relevante para esta discussão de planejamento da capacidade.  
  
|Tarefa recomendada|Descrição|Referência|  
|--------------------|---------------|-------------|  
|Entender os requisitos para implantar servidores de Federação do AD FS e proxies de servidor de Federação|Examine importantes requisitos de hardware e software necessários para implantar o servidor de Federação e proxies de servidor de Federação.|[Apêndice a: examinando os requisitos do AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Selecione o tipo de banco de dados de configuração de AD FS que você implantará em sua organização|Antes de você começar a usar dados de planejamento da capacidade nesta seção, primeiro você precisa determinar qual tipo de banco de dados de configuração do AD FS você implantará \(WID\) banco de dados interno do Windows ou um banco de dados de linguagem de consulta estruturada \(SQL\).|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[Considerações de topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
|Determinar o tipo de layout de topologia para usar com sua nova seleção de banco de dados de configuração do AD FS|Depois que você decidiu o tipo de banco de dados de configuração do AD FS para usar em sua implantação, será necessário considerar a topologia de implantação que melhor corresponda ao onde você precisará servidores de Federação do lugar e federação proxies de servidor dentro do ambiente de produção.|[Determinar a topologia de implantação do AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Entender a chave AD FS – relacionados planejamento da capacidade termos|Analise as definições de termos que são usados em toda a capacidade do AD FS planejando abordagem de planejamento da capacidade comuns.|Consulte a seção [termos de planejamento da capacidade do AD FS](Planning-for-AD-FS-Server-Capacity.md#bk_terms) neste tópico|  
  
Depois de analisarmos o conteúdo na tabela anterior, você agora pode concluir as tarefas de pré-requisito na tabela a seguir.  
  
|Tarefa de pré-requisito|Descrição|Referência|  
|---------------------|---------------|-------------|  
|Baixar a capacidade de FS AD planejamento planilha de dimensionamento|A planilha de dimensionamento de planejamento de capacidade do AD FS pode ajudar você a determinar o número de servidores de Federação necessários para uma implantação de farm do servidor de Federação do AD FS. Instruções sobre como usar essa planilha estão disponíveis no link fornecido abaixo para a próxima tarefa.|[Planilha de planejamento da capacidade do AD FS](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Coletar dados sobre o número de usuários que exigirá único sign\ \(SSO\) acesso para o aplicativo com reconhecimento de claims\ destino e os períodos de uso esperado pico associados a esse acesso|Esses dados de usuário que você coletar serão usados para os valores de entrada necessários dentro do contexto do AD FS capacidade planejamento dimensionamento planilha.|[Estimar o número de servidores de federação para sua organização](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|Planilha de planejamento da capacidade do AD FS para o Windows Server 2016|Planilha de planejamento atualizada para o Windows Server 2016|[AD FS Windows Server 2016 capacidade planejamento](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>Termos de planejamento do AD FS capacidade  
A tabela a seguir descreve os termos importantes que são usados com frequência nesta seção do guia de Design do AD FS de planejamento da capacidade. Para obter uma lista mais completa dos termos do AD FS, consulte [conceitos-chave de Noções básicas sobre AD FS](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Termo|Definição|  
|--------|--------------|  
|Usuários simultâneos|O número estimado de usuários que o esperado para enviar solicitações de serviço em um determinado período de tempo, geralmente um período de atividade de pico.|  
|Usuários ativos|O número médio aproximado de usuários que estão ativos em um sistema, mas não necessariamente enviar solicitações, durante um determinado período de tempo.|  
|Usuários definidos|Uma contagem de usuário máxima teórica, geralmente com base no número de usuários que definiu contas no sistema.|  
|Solicitações por segundo|O número de solicitações que seja enviado por clientes \ (quando falando sobre a carga de uma System \) ou processados pelos servidores \ (quando falando sobre throughput\ de servidor) em um segundo. Essa métrica é usada no planejamento de processador do servidor e capacidade de memória.|  
|Capacidade de resposta de servidor de destino e utilização|Métricas de sucesso que vinculado o intervalo de desempenho aceitável de servidor. Em geral, se capacidade de resposta fica abaixo ou utilização aumenta acima da meta, o sistema é considerado para ser sobrecarregado e mais capacidade é necessária.|  
|\(WID\) do banco de dados interno do Windows|O AD FS configuração banco de dados padrão que pode ser usado como uma alternativa para SQL Server em determinados implantações do AD FS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Ambiente de configuração usado durante o teste do AD FS  
Esta seção descreve o ambiente de configuração que a equipe de produto do AD FS usada para executar seus testes. A equipe usou o seguinte hardware do computador, software e a configuração de rede para coletar dados de desempenho e escalabilidade em testes do servidor de Federação:  
  
-   Dual Quad Core 2.27 gigahertz \(GHz\) \(8 cores\)  
  
-   16\ GB DE RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   Rede Gigabit  
  
> [!NOTE]  
> Embora 16 GB de RAM era usado no servidor de Federação durante o teste, um tamanho de memória mais moderado, como do 4 GB de RAM por servidor de Federação pode ser usado para a maioria das implantações do AD FS. As recomendações são fornecidas nessa AD FS planejamento da capacidade conteúdo juntamente com os resultados fornecidos pela planilha de planejamento de capacidade do AD FS se baseiam suposições que cada servidor de Federação usará aproximadamente 4 GB está de RAM para a maioria dos ambientes de produção do AD FS.  
  
A equipe do produto usada a seguinte configuração para coletar dados de desempenho e escalabilidade para o proxy do servidor de Federação testes:  
  
-   Dual \(4 cores\) Quad Core 2.24 GHz  
  
-   4\ GB DE RAM  
  
-   Windows Server 2008 R2 Enterprise Edition  
  
-   Rede Gigabit  
  
> [!NOTE]  
> Capacidade recomendações para servidores do AD FS podem variar consideravelmente, dependendo das especificações escolhida para a configuração do hardware e de rede para ser usado em um determinado ambiente. Como um ponto de referência, as diretrizes de dimensionamento fornecidas nesse conteúdo é baseada em um destino de utilização de 80% nos computadores mencionados anteriormente.  
  
## <a name="measure-ad-fs-server-capacity"></a>Capacidade do servidor Measure AD FS  
Normalmente, os componentes de hardware que afetam o desempenho do servidor e escalabilidade são a CPU, memória, disco e adaptadores de rede. Felizmente, cada um dos componentes do AD FS requer pouquíssima demanda de memória e espaço em disco. Conectividade de rede é um requisito óbvio. Portanto, os testes de carga são executados em servidores de Federação e proxies de servidor de Federação vamos nos concentrar nas duas áreas principais para medir a capacidade do servidor:  
  
-   **Pico solicitações do AD FS por segundo:** o número de solicitações sign\-in que são processadas por segundo em servidores de Federação. Essa medida pode ajudá-lo a determinar quantos usuários simultâneos podem fazer logon um determinado servidor. Você pode usar essa medida em conjunto com a medição de consumo de CPU para entender o efeito dessa medida no desempenho.  
  
-   **Consumo de CPU:** o percentual pelo qual CPU capacidade é medida. Essa medida pode ajudá-lo a determinar a carga total da CPU que ocorreram com base no número de solicitações de sign\-in recebidas por segundo.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Continuar a ler mais sobre o planejamento da capacidade do AD FS  
Depois de concluir as tarefas de pré-requisito e ter se familiarizar com os termos relacionados ao e requisitos de hardware, você pode usar a capacidade adicional de seguir planejar conteúdo para ajudá-lo a determinar o número recomendado de servidores do AD FS necessários para a sua implantação:  
  
-   [Planejamento de capacidade do servidor de Federação](Planning-for-Federation-Server-Capacity.md)  
  
-   [Planejamento de capacidade de Proxy do servidor de Federação](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Consulte também
[Guia de Design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
