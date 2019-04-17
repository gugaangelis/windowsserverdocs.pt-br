---
ms.assetid: 67d8a8d7-2fbd-4ed7-bb41-75769f942024
title: Configurar o monitoramento de desempenho
description: 
author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.author: billmath
ms.openlocfilehash: 6a7602cddcaee274d42213cd9365f6d1722dab79
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/03/2017
---
# <a name="configure-performance-monitoring"></a>Configurar o monitoramento de desempenho

>Aplica-se a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012
  
## <a name="bkmk_ConfigurePerfMon"></a>  
AD FS inclui seus próprios contadores de desempenho dedicado para ajudar a monitorar o desempenho dos servidores de Federação e computadores de proxy do servidor de Federação. Para usar o Monitor de desempenho para monitorar o desempenho de seus servidores do AD FS, é útil criar um novo conjunto de coletor de dados e adicione os contadores do AD FS esse modo de exibição. O procedimento a seguir descreve como configurar o monitoramento de desempenho do AD FS.  
  
#### <a name="to-configure-performance-monitoring-for-ad-fs-using-performance-monitor"></a>Para configurar o monitoramento de desempenho do AD FS usando o Monitor de desempenho  
  
1.  Sobre o **iniciar** de tela, digite **Monitor de desempenho**, e pressione ENTER.  
  
2.  Na árvore de console, expanda **conjuntos de coletor de dados**, clique right\ **definido pelo usuário**, aponte para **nova**e clique em **conjunto de Coletores de dados**.  
  
    Criar novos dados coletor Assistente de configuração é exibida.  
  
3.  Em **criar conjunto de Coletores de dados novos**, para **nome** digite um nome para o novo conjunto de coletor de dados \ (como "Desempenho AD FS" \), clique em **criar manualmente \(Advanced\)**e clique em **próxima**.  
  
4.  Para o tipo de dados a serem incluídos, verifique se **criar logs de dados** é selecionado e, em seguida, marque as caixas de seleção para os seguintes tipos de dados: **contador de desempenho**, **dados de rastreamento de eventos**, **informações de configuração do sistema**.  
  
5.  Contadores de desempenho, expanda **AD FS** no **contadores disponíveis** listar e clique em **adicionar**.  
  
    Os contadores de desempenho do AD FS devem aparecer no **adicionado contadores** lista.  
  
6.  Quando você for solicitado a adicionar provedores de rastreamento de eventos, clique em **adicionar**, selecione **AD FS eventos** e **AD FS rastreamento** da lista de provedores.  
  
7.  Quando você for solicitado a adicionar chaves do registro para monitorar, clique em **próxima**.  
  
8.  Quando você for solicitado a especificar o local para salvar os dados de desempenho, você pode aceitar o local padrão \ (**%systemdrive%\\PerfLogs\\Admin\\***< data\_collector\_set >*e clique em **próxima**.  
  
9. Quando você for solicitado a criar o conjunto de coletor de dados, selecione **salve e feche**e clique em **concluir**.  
  
    O novo conjunto de coletor de dados aparece na árvore de console no **definido pelo usuário** nó.  
  
10. Use as etapas a seguir para trabalhar com os contadores de desempenho do AD FS:  
  
    -   Para começar usando relacionados à publicidade FS\ contadores de monitoramento de desempenho, clique right\ o coletor de dados definir que você adicionou \ (por exemplo, "Desempenho AD FS" \) e, em seguida, clique em **iniciar**.  
  
    -   Para criar um relatório para exibir o resultados de monitoramento de desempenho, clique right\ o coletor de dados defina que você adicionou \ (por exemplo, "Desempenho AD FS" \) e, em seguida, clique em **relatório mais recente**.  
  
    -   Para finalizar uma captura de dados de desempenho para que você possa ver o relatório mais recente, clique right\ o coletor de dados definir que você adicionou \ (por exemplo, "Desempenho AD FS" \) e, em seguida, clique em **parar**.  
  
    O relatório mais recente é adicionado e numerado automaticamente \ (começando em 000001\) sob o **Report\\User definido***\ \ < data\_collector\_set >* nó na árvore de console.  
  
## <a name="ad-fs-performance-counters"></a>Contadores de desempenho do AD FS  
A tabela a seguir lista os contadores de desempenho do AD FS e descreve como eles são úteis para monitorar a atividade está relacionada a um servidor de Federação ou o proxy do servidor de Federação.  
  
|Contador|Descrição|Pode ser usado em: 
|-----------|---------------|------------------- 
|Solicitações de token|Monitora o número de solicitações de token enviado para o servidor de Federação incluindo SSOAuth solicitações de token.|Servidores de Federação 
|Token Requests\/s|Monitora o número de solicitações de token enviado para o servidor de Federação incluindo solicitações de token SSOAuth por segundo.|Servidores de Federação  
|Solicitações de metadados de Federação|Monitora o número de solicitações de metadados de Federação recebidas enviado para o servidor de Federação.|Servidores de Federação  
|Federação Requests\ de metadados/s|Monitora o número de Federação metadados solicitações recebidas por segundo que são enviados para o servidor de Federação.|Servidores de Federação  
|Solicitações de resolução de artefato|Monitora o número de Federação metadados solicitações recebidas por segundo que são enviados para o servidor de Federação.|Servidores de Federação  
|Resolução de artefato Requests\/s|Monitora o número de solicitações para o ponto de extremidade de resolução artefato por segundo que são enviados para o servidor de Federação.|Servidores de Federação  
|Solicitações de proxy|Monitora o número de solicitações enviadas para o proxy do servidor de Federação.|Proxies de servidor de Federação  
|Proxy Requests\/s|Monitora o número de solicitações de entrada por segundo que são enviadas para o proxy do servidor de Federação.|Proxies de servidor de Federação  
|Solicitações MEX de proxy|Monitora o número de solicitações de troca de metadados WS\ \(MEX\) recebidas que são enviadas para o proxy do servidor de Federação.|Proxies de servidor de Federação 
|Proxy MEX Requests\/s|Monitora o número de solicitações de MEX recebidas por segundo que são enviadas para o proxy do servidor de Federação.|Proxies de servidor de Federação  
  

