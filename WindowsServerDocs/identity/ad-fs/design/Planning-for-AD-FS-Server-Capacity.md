---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: Planejando a capacidade do servidor do AD FS
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 932babf30e09b567e5e0e406e20454de995820b1
ms.sourcegitcommit: f305bc5f1c5a44dac62f4288450af19f351f9576
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87118621"
---
# <a name="planning-for-ad-fs-server-capacity"></a>Planejando a capacidade do servidor do AD FS


  
> [!NOTE]  
> O conteúdo fornecido neste tópico não reflete os testes reais que foram executados em servidores que executam o Windows Server 2012. Este tópico será atualizado após a realização dos testes necessários.  
  
Planejamento de capacidade para Serviços de Federação do Active Directory (AD FS) \( AD FS \) é o processo de previsão de períodos de pico de uso para seu serviço de Federação e planejamento ou expansão \- da implantação do AD FS Server para atender a esses requisitos de carga.  
  
Esta seção descreve as diretrizes de implantação para as funções de proxy do servidor de Federação e do servidor de Federação e baseia-se no teste de laboratório realizado pela equipe de produto AD FS na Microsoft. O objetivo deste conteúdo é ajudá-lo a:  
  
-   Estimar rigorosamente as necessidades de hardware para a implantação de AD FS específica da sua organização, como o número de servidores AD FS.  
  
-   Projetar com precisão o uso de pico esperado para \- solicitações de entrada, planejar o crescimento e garantir que sua implantação de AD FS seja capaz de lidar com o pico de uso esperado.  
  
Antes de continuar a ler este conteúdo sobre planejamento de capacidade, é recomendável concluir primeiro as tarefas na ordem mostrada nas duas tabelas a seguir. Na primeira tabela, são apresentados links para tarefas recomendadas que ajudarão a fornecer contexto relevante a esta discussão sobre planejamento de capacidade.  
  
|Tarefa recomendada|Descrição|Referência|  
|--------------------|---------------|-------------|  
|Entender os requisitos para implantar AD FS servidores de Federação e proxies de servidor de Federação|Examine importantes requisitos de hardware e software necessários para implantar o servidor de federação e os proxies do servidor de federação.|[Apêndice A: Como examinar os requisitos do AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Selecione o tipo de banco de dados de configuração AD FS que será implantado em sua organização|Antes de começar a usar os dados de planejamento de capacidade nesta seção, primeiro você precisa determinar qual AD FS tipo de banco de dado de configuração que será implantado, o wid do banco de dados interno do Windows \( \) ou um \( banco de dados SQL linguagem SQL \) .|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<p>[Considerações sobre a topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
|Determinar o tipo de layout de topologia a ser usado com sua nova seleção de banco de dados de configuração do AD FS.|Depois de ter decidido o tipo de banco de dados de configuração do AD FS a ser usado na sua implantação, considere a topologia de implantação que melhor corresponde ao local em que estarão os servidores de federação e proxies dos servidores de federação no ambiente de produção.|[Determinar a topologia de implantação do AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Entender os termos de planejamento de capacidade relacionados ao AD FS-chave|Examine as definições de termos de planejamento de capacidade comuns que são usados em toda a discussão de planejamento de capacidade AD FS.|Confira a seção intitulada [Termos do planejamento de capacidade do AD FS](Planning-for-AD-FS-Server-Capacity.md#bk_terms) neste tópico|  
  
Depois de examinar o conteúdo da tabela anterior, você poderá concluir as tarefas de pré-requisito da próxima tabela.  
  
|Tarefa de pré-requisito|Descrição|Referência|  
|---------------------|---------------|-------------|  
|Baixar a planilha de dimensionamento de planejamento de capacidade AD FS|A planilha de dimensionamento de planejamento de capacidade AD FS pode ajudá-lo a determinar o número de servidores de Federação necessários para uma implantação AD FS farm de servidores de Federação. Instruções de uso dessa planilha estão disponíveis no link fornecido abaixo para a próxima tarefa.|[Planilha de planejamento de capacidade AD FS](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Coletar dados sobre o número de usuários que precisarão de acesso SSO de logon único \- \( \) para o aplicativo de reconhecimento de declarações de destino \- e os períodos de pico de uso esperados associados a este acesso|Esses dados do usuário serão usados para os valores de entrada necessários no contexto da planilha de dimensionamento do planejamento de capacidade do AD FS.|[Estimar o número de servidores de federação da sua organização](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|Planilha de planejamento de capacidade AD FS para o Windows Server 2016|Planilha de planejamento atualizada para o Windows Server 2016|[AD FS o planejamento de capacidade do Windows Server 2016](https://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="ad-fs-capacity-planning-terms"></a><a name="bk_terms"></a>Termos do planejamento de capacidade do AD FS  
A tabela a seguir descreve os termos importantes que são usados com frequência nesta seção planejamento de capacidade do guia de design de AD FS. Para obter uma lista mais completa de termos de AD FS, consulte [noções básicas sobre conceitos de AD FS de chave](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Termo|Definição|  
|--------|--------------|  
|Usuários simultâneos|O número estimado de usuários que devem enviar solicitações ao serviço em determinado período, normalmente um período de pico de atividade.|  
|Usuários ativos|O número médio aproximado de usuários que estão ativos em um sistema, mas não necessariamente enviando solicitações, durante determinado período.|  
|Usuários definidos|Uma contagem teórica máxima de usuários, normalmente com base no número de usuários que têm contas definidas no sistema.|  
|Solicitações por segundo|O número de solicitações enviadas por clientes \( ao falar sobre a carga em um sistema \) ou processada pelos servidores \( ao falar sobre a taxa de transferência do servidor \) em um segundo. Essa métrica é usada no planejamento do processador do servidor e da capacidade de memória.|  
|Capacidade de resposta e utilização do servidor de destino|Métrica de sucesso que limita o intervalo aceitável de desempenho do servidor. Geralmente, se a capacidade de resposta ficar abaixo do destino ou a utilização ficar acima dele, o sistema será considerado como sobrecarregado e mais capacidade será necessária.|  
|WID do banco de dados interno do Windows \(\)|O banco de dados de configuração padrão AD FS que pode ser usado como uma alternativa para SQL Server em determinadas implantações de AD FS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Ambiente de configuração usado durante os testes do AD FS  
Esta seção descreve o ambiente de configuração que a equipe de produto AD FS usou para executar seus testes. A equipe usou as seguintes configurações de computador para hardware, software e rede para coletar dados de desempenho e escalabilidade nos testes do servidor de federação:  
  
-   Dual Quad Core 2,27 gigahertz \( GHz \) \( 8 núcleos\)  
  
-   16 \- GB de RAM  
  
-   Windows Server 2008 R2, Enterprise Edition  
  
-   Rede Gigabit  
  
> [!NOTE]  
> Embora 16 GB de RAM tenha sido usado no servidor de Federação durante o teste, um tamanho de memória mais moderado, como 4 GB de RAM por servidor de Federação, pode ser usado para a maioria AD FS implantações. As recomendações fornecidas neste AD FS o conteúdo de planejamento de capacidade junto com os resultados fornecidos pela AD FS planilha de planejamento de capacidade baseiam-se em suposições que cada servidor de Federação usará aproximadamente 4GB's de RAM para a maioria dos ambientes de produção de AD FS.  
  
A equipe de produto usou a seguinte configuração para coletar dados de desempenho e escalabilidade para os testes do proxy do servidor de federação:  
  
-   Dual Quad Core de 2,24 GHz \( 4 núcleos\)  
  
-   4 \- GB de RAM  
  
-   Windows Server 2008 R2, Enterprise Edition  
  
-   Rede Gigabit  
  
> [!NOTE]  
> As recomendações de capacidade para servidores AD FS podem variar consideravelmente, dependendo das especificações escolhidas para a configuração de rede e de hardware a ser usada em um determinado ambiente. Como ponto de referência, as diretrizes de dimensionamento fornecidas neste conteúdo se baseiam em uma meta de utilização de 80% nos computadores mencionados anteriormente.  
  
## <a name="measure-ad-fs-server-capacity"></a>Medir a capacidade do servidor do AD FS  
Tipicamente, os componentes de hardware que afetam o desempenho e a escalabilidade do servidor são a CPU, a memória, o disco e os adaptadores de rede. Felizmente, cada um dos componentes de AD FS requer muito pouca demanda em memória e espaço em disco. A conectividade de rede é um requisito óbvio. Portanto, os testes de carga que são realizados nos servidores de federação e proxies dos servidores de federação se concentram em duas áreas principais para medir a capacidade do servidor:  
  
-   **Pico de solicitações de AD FS por segundo:** O número de solicitações de entrada \- processadas por segundo em servidores de Federação. Essa medida pode ajudá-lo a determinar quantos usuários simultâneos podem se conectar a determinado servidor. Você pode usar essa medida em conjunto com a medida de consumo de CPU para entender o efeito dela sobre o desempenho.  
  
-   **Consumo de CPU:** O percentual pelo qual a capacidade da CPU é medida. Essa medida pode ajudá-lo a determinar a carga de CPU geral que ocorreu com base no número de solicitações de entrada recebidas \- por segundo.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Leia mais sobre o planejamento de capacidade do AD FS  
Depois de concluir as tarefas de pré-requisito e familiarizar-se com os termos e requisitos de hardware relacionados, você pode usar o seguinte conteúdo de planejamento de capacidade adicional para ajudá-lo a determinar o número recomendado de servidores de AD FS necessários para sua implantação:  
  
-   [Como planejar a capacidade do servidor de federação](Planning-for-Federation-Server-Capacity.md)  
  
-   [Como planejar a capacidade do proxy do servidor de federação](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Consulte Também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
