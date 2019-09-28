---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: Configurar monitoramento de desempenho
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 37dd52b8771eda695069dd996fbd920e31f80ef1
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71359812"
---
# <a name="configure-performance-monitoring"></a>Configurar monitoramento de desempenho
  
## <a name="bkmk_ConfigurePerfMon"></a>  
O AD FS inclui seus próprios contadores de desempenho dedicados para ajudá-lo a monitorar o desempenho de servidores de Federação e de computadores proxy de servidor de Federação. Para usar o monitor de desempenho para monitorar o desempenho de seus servidores de AD FS, é útil criar um novo conjunto de coletores de dados e adicionar os contadores de AD FS a essa exibição. O procedimento a seguir descreve como configurar o monitoramento de desempenho para AD FS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Para configurar o monitoramento de desempenho para AD FS usando o monitor de desempenho  
  
1. Na tela **Iniciar** , digite **Monitor de desempenho**e pressione Enter.  
  
2. Na árvore de console, expanda **conjuntos de coletores de dados**, clique com o botão\-direito do mouse em **definido pelo usuário**, aponte para **novo**e clique em conjunto do **coletor de dados**.  
  
   O assistente para criar novo conjunto de coletores de dados é exibido.  
  
3. Em **criar novo conjunto de coletores de dados**, para **nome** , digite um nome para o \(novo conjunto de coletores de\)dados, como "AD FS desempenho  **\(\)** ", clique em criar manualmente e, em seguida, clique em  **Em seguida**.  
  
4. Para o tipo de dados a ser incluído, verifique se **criar logs de dados** está selecionado e, em seguida, clique nas caixas de seleção para os seguintes tipos de dados: **Contador de desempenho**, **dados de rastreamento de eventos**, informações de configuração do **sistema**.  
  
5. Para contadores de desempenho, expanda **AD FS** na lista **contadores disponíveis** e clique em **Adicionar**.  
  
   Os contadores de desempenho AD FS devem aparecer na lista de **contadores adicionados** .  
  
6. Quando for solicitado a adicionar provedores de rastreamento de eventos, clique em **Adicionar**, selecione **AD FS eventos** e **AD FS rastreamento** na lista de provedores.  
  
7. Quando for solicitado a adicionar chaves do registro para monitorar, clique em **Avançar**.  
  
8. Quando for solicitado a especificar o local para salvar os dados de desempenho, você poderá aceitar o local padrão \( **% systemdrive% \\PerfLogs @ no__t-3Admin @ no__t-4** _< data @ no__t-6collector @ no__t-7set >_ e clique em **Em seguida**.  
  
9. Quando for solicitado a criar o conjunto de coletores de dados, selecione **salvar e fechar**e clique em **concluir**.  
  
    O novo conjunto de coletores de dados aparece na árvore de console no nó **definido pelo usuário** .  
  
10. Use as etapas a seguir para trabalhar com os contadores de desempenho AD FS:  
  
    -   Para iniciar o monitoramento de desempenho\-usando AD FS contadores relacionados\-, clique com o botão direito do mouse \(no conjunto de coletores de\)dados que você adicionou, como "desempenho de AD FS", e clique em **Iniciar**.  
  
    -   Para criar um relatório para exibir os resultados de monitoramento de desempenho\-, clique com o botão direito do mouse \(no conjunto de coletores de\)dados que você adicionou, como "AD FS desempenho", e clique em **relatório mais recente**.  
  
    -   Para encerrar uma captura de dados de desempenho para que você possa exibir o relatório mais recente\-, clique com o botão direito do mouse \(no conjunto de coletores de\)dados que você adicionou, como "AD FS desempenho", e clique em **parar**.  
  
    O relatório mais recente é adicionado e numerado \(automaticamente a partir\) de 000001 **no\\relatório definido pelo**<em>\\usuário\_<\_conjunto de coletores de dados ></em> nó em a árvore de console.  
  
## <a name="ad-fs-performance-counters"></a>Contadores de desempenho de AD FS  
A tabela a seguir lista os contadores de desempenho AD FS e descreve como eles são úteis para monitorar a atividade relacionada a um servidor de Federação ou proxy de servidor de Federação.  
  
|Contador|Descrição|Pode ser usado em: 
|-----------|---------------|------------------- 
|Solicitações de Token|Monitora o número de solicitações de token enviadas ao servidor de Federação, incluindo solicitações de token SSOAuth.|Servidores de Federação 
|Solicitações\/de token s|Monitora o número de solicitações de token enviadas ao servidor de Federação, incluindo solicitações de token SSOAuth por segundo.|Servidores de Federação  
|Solicitações de Metadados de Federação|Monitora o número de solicitações de metadados de Federação de entrada enviadas ao servidor de Federação.|Servidores de Federação  
|Solicitações\/de metadados de Federação s|Monitora o número de solicitações de metadados de Federação recebidas por segundo que são enviadas para o servidor de Federação.|Servidores de Federação  
|Solicitações de Resolução Artefato|Monitora o número de solicitações de metadados de Federação recebidas por segundo que são enviadas para o servidor de Federação.|Servidores de Federação  
|Solicitações\/de resolução de artefato s|Monitora o número de solicitações para o ponto de extremidade de resolução de artefatos por segundo que são enviadas para o servidor de Federação.|Servidores de Federação  
|Solicitações de proxy|Monitora o número de solicitações de entrada enviadas ao proxy do servidor de Federação.|Proxies de servidor de Federação  
|Solicitações\/de proxy s|Monitora o número de solicitações de entrada por segundo que são enviadas para o proxy do servidor de Federação.|Proxies de servidor de Federação  
|Solicitações de proxy MEX|Monitora o número de solicitações do\-Exchange \(MEX\) de entrada do WS Metadata enviadas ao proxy do servidor de Federação.|Proxies de servidor de Federação 
|Solicitações\/de proxy MEX s|Monitora o número de solicitações MEX de entrada por segundo que são enviadas para o proxy do servidor de Federação.|Proxies de servidor de Federação  
  

