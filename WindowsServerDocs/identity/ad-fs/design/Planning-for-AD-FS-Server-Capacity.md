---
ms.assetid: ef91f1d8-2991-4d90-b687-5fa189737c88
title: Planejando a capacidade do servidor do AD FS
description: ''
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 0191c822ec068c5486a1b0d5da4c1ae2ee9e4d31
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66191100"
---
# <a name="planning-for-ad-fs-server-capacity"></a>Planejando a capacidade do servidor do AD FS


  
> [!NOTE]  
> O conteúdo fornecido neste tópico não reflete os testes reais que foi executada em servidores que executam o Windows Server 2012. Este tópico será atualizado após a realização dos testes necessários.  
  
Planejamento de capacidade para os serviços de Federação do Active Directory \(do AD FS\) é o processo de previsão de períodos de pico de uso para seu serviço de Federação e planejar ou escalar\-verticalmente sua implantação de servidor do AD FS para atender aos de carga requisitos.  
  
Esta seção descreve as diretrizes de implantação para o servidor de Federação e funções de proxy do servidor de Federação e baseia-se nos testes de laboratório que foi executada pela equipe de produto do AD FS na Microsoft. O objetivo deste conteúdo é ajudá-lo a:  
  
-   Estime cuidadosamente as necessidades de hardware para a implantação de AD FS específica da sua organização, como o número de servidores do AD FS.  
  
-   Com precisão o uso de pico esperada para uma entrada de projeto\-em solicitações, planeje o crescimento e certifique-se de que sua implantação do AD FS é capaz de acomodar esse pico de uso.  
  
Antes de continuar a ler este conteúdo sobre planejamento de capacidade, é recomendável concluir primeiro as tarefas na ordem mostrada nas duas tabelas a seguir. Na primeira tabela, são apresentados links para tarefas recomendadas que ajudarão a fornecer contexto relevante a esta discussão sobre planejamento de capacidade.  
  
|Tarefa recomendada|Descrição|Referência|  
|--------------------|---------------|-------------|  
|Entender os requisitos para implantar servidores de Federação do AD FS e proxies de servidor de Federação|Examine importantes requisitos de hardware e software necessários para implantar o servidor de federação e os proxies do servidor de federação.|[Apêndice A: Como examinar os requisitos do AD FS](Appendix-A--Reviewing-AD-FS-Requirements.md)|  
|Selecione o tipo de banco de dados de configuração de AD FS que você implantará na sua organização|Antes de começar a usar dados de planejamento de capacidade nesta seção, você primeiro precisa determinar qual configuração do AD FS tipo implantará, qualquer banco de dados interno do Windows do banco de dados \(WID\) ou uma linguagem de consulta estruturada \(SQL\) banco de dados.|[A função do banco de dados de configuração do AD FS](../../ad-fs/technical-reference/The-Role-of-the-AD-FS-Configuration-Database.md);<br /><br />[Considerações sobre a topologia de implantação do AD FS](AD-FS-Deployment-Topology-Considerations.md)|  
|Determinar o tipo de layout de topologia a ser usado com sua nova seleção de banco de dados de configuração do AD FS.|Depois de ter decidido o tipo de banco de dados de configuração do AD FS a ser usado na sua implantação, considere a topologia de implantação que melhor corresponde ao local em que estarão os servidores de federação e proxies dos servidores de federação no ambiente de produção.|[Determinar a topologia de implantação do AD FS](Determine-Your-AD-FS-Deployment-Topology.md)|  
|Entender a chave do AD FS relacionados ao planejamento de capacidade termos|Examine as definições dos termos que são usados em toda a discussão de planejamento de capacidade do AD FS de planejamento de capacidade comuns.|Confira a seção intitulada [Termos do planejamento de capacidade do AD FS](Planning-for-AD-FS-Server-Capacity.md#bk_terms) neste tópico|  
  
Depois de examinar o conteúdo da tabela anterior, você poderá concluir as tarefas de pré-requisito da próxima tabela.  
  
|Tarefa de pré-requisito|Descrição|Referência|  
|---------------------|---------------|-------------|  
|Baixar a planilha de dimensionamento do planejamento de capacidade do AD FS|A planilha de dimensionamento do planejamento de capacidade do AD FS pode ajudar você a determinar o número de servidores de Federação necessários para uma implantação de farm de servidor de Federação do AD FS. Instruções de uso dessa planilha estão disponíveis no link fornecido abaixo para a próxima tarefa.|[Planilha de planejamento de capacidade do AD FS](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacityPlanning.xlsx)|  
|Coletar dados sobre o número de usuários que precisarão de logon único\-na \(SSO\) acesso às declarações de destino\-aplicativo com reconhecimento e os períodos de uso de pico esperada associados com esse acesso|Esses dados do usuário serão usados para os valores de entrada necessários no contexto da planilha de dimensionamento do planejamento de capacidade do AD FS.|[Estimar o número de servidores de federação para sua organização](Planning-for-Federation-Server-Capacity.md#bk_estimatefs)|  
|Planilha de planejamento de capacidade do AD FS do Windows Server 2016|Planilha de planejamento atualizada para o Windows Server 2016|[AD FS do Windows planejamento de capacidade do Server 2016](http://adfsdocs.blob.core.windows.net/adfs/ADFSCapacity2016.xlsx)  
  
## <a name="bk_terms"></a>Termos do planejamento de capacidade do AD FS  
A tabela a seguir descreve os termos importantes que são usados com frequência essa seção do guia de Design do AD FS de planejamento de capacidade. Para obter uma lista mais completa de termos do AD FS, consulte [Understanding Key AD FS Concepts](../../ad-fs/technical-reference/Understanding-Key-AD-FS-Concepts.md).  
  
|Termo|Definição|  
|--------|--------------|  
|Usuários simultâneos|O número estimado de usuários que devem enviar solicitações ao serviço em determinado período, normalmente um período de pico de atividade.|  
|Usuários ativos|O número médio aproximado de usuários que estão ativos em um sistema, mas não necessariamente enviando solicitações, durante determinado período.|  
|Usuários definidos|Uma contagem teórica máxima de usuários, normalmente com base no número de usuários que têm contas definidas no sistema.|  
|Solicitações por segundo|O número de solicitações ou enviadas por clientes \(quando falamos sobre a carga em um sistema\) ou processadas por servidores \(quando falamos sobre a taxa de transferência do servidor\) em um segundo. Essa métrica é usada no planejamento do processador do servidor e da capacidade de memória.|  
|Capacidade de resposta e utilização do servidor de destino|Métrica de sucesso que limita o intervalo aceitável de desempenho do servidor. Geralmente, se a capacidade de resposta ficar abaixo do destino ou a utilização ficar acima dele, o sistema será considerado como sobrecarregado e mais capacidade será necessária.|  
|Banco de dados interno do Windows \(WID\)|O AD FS configuration banco de dados padrão que pode ser usado como uma alternativa para o SQL Server em determinadas implantações do AD FS.|  
  
## <a name="configuration-environment-used-during-ad-fs-testing"></a>Ambiente de configuração usado durante os testes do AD FS  
Esta seção descreve o ambiente de configuração que a equipe de produto do AD FS usada para executar seus testes. A equipe usou as seguintes configurações de computador para hardware, software e rede para coletar dados de desempenho e escalabilidade nos testes do servidor de federação:  
  
-   Gigahertz dual de Quad Core 2.27 \(GHz\) \(8 núcleos\)  
  
-   16\-GB DE RAM  
  
-   Windows Server 2008 R2, Enterprise Edition  
  
-   Rede Gigabit  
  
> [!NOTE]  
> Embora a 16 GB de RAM foi usado no servidor de Federação durante o teste, um tamanho de memória mais moderado, como 4 GB de RAM por servidor de Federação pode ser usado para a maioria das implantações do AD FS. As recomendações que são fornecidas nessa AD FS planejamento de capacidade do conteúdo, juntamente com os resultados apresentados pela planilha de planejamento de capacidade do AD FS se baseiam nas suposições que cada servidor de Federação usará aproximadamente 's de 4 GB de RAM para a maioria de produção do AD FS ambientes.  
  
A equipe de produto usou a seguinte configuração para coletar dados de desempenho e escalabilidade para os testes do proxy do servidor de federação:  
  
-   Dual Core de Quad 2,24 GHz \(4 núcleos\)  
  
-   4\-GB DE RAM  
  
-   Windows Server 2008 R2, Enterprise Edition  
  
-   Rede Gigabit  
  
> [!NOTE]  
> Recomendações de capacidade para servidores do AD FS podem variar consideravelmente, dependendo das especificações escolhidas para a configuração de hardware e de rede a ser usado em um determinado ambiente. Como ponto de referência, as diretrizes de dimensionamento fornecidas neste conteúdo se baseiam em uma meta de utilização de 80% nos computadores mencionados anteriormente.  
  
## <a name="measure-ad-fs-server-capacity"></a>Medir a capacidade do servidor do AD FS  
Tipicamente, os componentes de hardware que afetam o desempenho e a escalabilidade do servidor são a CPU, a memória, o disco e os adaptadores de rede. Felizmente, cada um dos componentes do AD FS requer muito pouca demanda de memória e espaço em disco. A conectividade de rede é um requisito óbvio. Portanto, os testes de carga que são realizados nos servidores de federação e proxies dos servidores de federação se concentram em duas áreas principais para medir a capacidade do servidor:  
  
-   **Solicitações de pico do AD FS por segundo:** O número de entrada\-nas solicitações que são processadas por segundo nos servidores de Federação. Essa medida pode ajudá-lo a determinar quantos usuários simultâneos podem se conectar a determinado servidor. Você pode usar essa medida em conjunto com a medida de consumo de CPU para entender o efeito dela sobre o desempenho.  
  
-   **Consumo de CPU:** a porcentagem pela qual a capacidade da CPU é medida. Essa medida pode ajudá-lo a determinar a carga total de CPU que ocorreu com base no número de sinal de entrada\-em solicitações por segundo.  
  
## <a name="continue-reading-more-about-ad-fs-capacity-planning"></a>Leia mais sobre o planejamento de capacidade do AD FS  
Depois de ter concluído as tarefas de pré-requisito e estiver familiarizado com termos relacionados e os requisitos de hardware, você pode usar a seguinte conteúdo de planejamento de capacidade adicional para ajudá-lo a determinar o número recomendado de servidores do AD FS necessários para seu implantação:  
  
-   [Como planejar a capacidade do servidor de federação](Planning-for-Federation-Server-Capacity.md)  
  
-   [Como planejar a capacidade do proxy do servidor de federação](Planning-for-Federation-Server-Proxy-Capacity.md)  
  
## <a name="see-also"></a>Consulte também
[Guia de design do AD FS no Windows Server 2012](AD-FS-Design-Guide-in-Windows-Server-2012.md)
