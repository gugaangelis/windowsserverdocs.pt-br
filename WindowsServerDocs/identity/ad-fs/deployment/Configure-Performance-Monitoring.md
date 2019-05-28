---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: Configurar monitoramento de desempenho
description: ''
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 5426ea929037e59d2105fb2b3b06d4ebfdb7a577
ms.sourcegitcommit: 0b5fd4dc4148b92480db04e4dc22e139dcff8582
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66192290"
---
# <a name="configure-performance-monitoring"></a>Configurar monitoramento de desempenho
  
## <a name="bkmk_ConfigurePerfMon"></a>  
O AD FS inclui seus próprios contadores de desempenho dedicados para ajudá-lo a monitorar o desempenho dos servidores de Federação e computadores de proxy do servidor de Federação. Para usar o Monitor de desempenho para monitorar o desempenho dos servidores do AD FS, é útil criar um novo conjunto de Coletores de dados e adicionar os contadores do AD FS para essa exibição. O procedimento a seguir descreve como configurar o monitoramento de desempenho para o AD FS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Para configurar o monitoramento de desempenho do AD FS usando o Monitor de desempenho  
  
1.  Sobre o **iniciar** tela, digite **Monitor de desempenho**, e pressione ENTER.  
  
2.  Na árvore de console, expanda **conjuntos de Coletores de dados**certo\-clique em **definidas pelo usuário**, aponte para **novo**e, em seguida, clique em **conjunto de Coletores de dados** .  
  
    Dados de coletor definir Assistente para criar novo é exibido.  
  
3.  Na **criar conjunto de Coletores de dados novos**, para **nome** digite um nome para o novo conjunto de Coletores de dados \(, como "O desempenho do AD FS"\), clique em **criar manualmente \( Advanced\)** e, em seguida, clique em **próxima**.  
  
4.  Para o tipo de dados a serem incluídos, verifique **criar logs de dados** está selecionado e, em seguida, clique nas caixas de seleção para os seguintes tipos de dados: **Contador de desempenho**, **dados de rastreamento de eventos**, **informações de configuração do sistema**.  
  
5.  Contadores de desempenho, expanda **do AD FS** na **contadores disponíveis** e, em seguida, clique **adicionar**.  
  
    Os contadores de desempenho do AD FS devem aparecer na **adicionado contadores** lista.  
  
6.  Quando você for solicitado a adicionar provedores de rastreamento de eventos, clique em **Add**, selecione **AD FS Eventing** e **rastreamento do AD FS** da lista de provedores.  
  
7.  Quando você for solicitado a adicionar as chaves do registro para monitorar, clique em **próxima**.  
  
8.  Quando você for solicitado a especificar o local para salvar os dados de desempenho, você pode aceitar o local padrão \( * *% systemdrive %\\PerfLogs\\Admin\\* * * < data\_coletor\_definir >* e, em seguida, clique em **próximo**.  
  
9. Quando você for solicitado a criar o conjunto de Coletores de dados, selecione **salve e feche**e, em seguida, clique em **concluir**.  
  
    O novo conjunto de Coletores de dados é exibido na árvore de console sob o **definidas pelo usuário** nó.  
  
10. Use as etapas a seguir para trabalhar com os contadores de desempenho do AD FS:  
  
    -   Para iniciar o monitoramento de desempenho usando o AD FS\-relacionado aos contadores, à direita\-clique em conjunto de Coletores de dados que você adicionou \(como "O desempenho do AD FS"\)e, em seguida, clique em **iniciar**.  
  
    -   Para criar um relatório para exibir o resultados de monitoramento de desempenho, com o botão direito\-clique em conjunto de Coletores de dados que você adicionou \(como "O desempenho do AD FS"\)e, em seguida, clique em **relatório mais recente**.  
  
    -   Para encerrar uma captura de dados de desempenho para que você possa exibir o relatório mais recente, com o botão direito\-clique em conjunto de Coletores de dados que você adicionou \(como "O desempenho do AD FS"\)e, em seguida, clique em **parar**.  
  
    O relatório mais recente é adicionado e numerado automaticamente \(começando em 000001\) sob o **relatório\\definido pelo usuário *\\< data\_coletor\_definido >* nó na árvore de console.  
  
## <a name="ad-fs-performance-counters"></a>Contadores de desempenho do AD FS  
A tabela a seguir lista os contadores de desempenho do AD FS e descreve como eles são úteis para monitorar a atividade que está relacionado a um servidor de Federação ou proxy do servidor de Federação.  
  
|Contador|Descrição|Pode ser usado em: 
|-----------|---------------|------------------- 
|Solicitações de Token|Monitora o número de solicitações de token enviado ao servidor de federação, incluindo solicitações de token SSOAuth.|Servidores de Federação 
|Solicitações de token\/s|Monitora o número de solicitações de token enviado ao servidor de federação, incluindo solicitações de token SSOAuth por segundo.|Servidores de Federação  
|Solicitações de Metadados de Federação|Monitora o número de solicitações de metadados de Federação entrada enviados para o servidor de Federação.|Servidores de Federação  
|Solicitações de metadados de Federação\/s|Monitora o número de Federação metadados solicitações recebidas por segundo que são enviadas para o servidor de Federação.|Servidores de Federação  
|Solicitações de Resolução Artefato|Monitora o número de Federação metadados solicitações recebidas por segundo que são enviadas para o servidor de Federação.|Servidores de Federação  
|Solicitações de resolução de artefato\/s|Monitora o número de solicitações para o ponto de extremidade de resolução de artefato por segundo que são enviadas para o servidor de Federação.|Servidores de Federação  
|Solicitações de proxy|Monitora o número de solicitações de entrada enviados para o proxy do servidor de Federação.|Proxies de servidor de Federação  
|Solicitações de proxy\/s|Monitora o número de solicitações recebidas por segundo que são enviadas para o proxy do servidor de Federação.|Proxies de servidor de Federação  
|Solicitações de MEX do proxy|Monitora o número de entrada WS\-troca de metadados \(MEX\) solicitações que são enviadas para o proxy do servidor de Federação.|Proxies de servidor de Federação 
|Proxy nas solicitações MEX\/s|Monitora o número de solicitações MEX recebidas por segundo que são enviadas para o proxy do servidor de Federação.|Proxies de servidor de Federação  
  

