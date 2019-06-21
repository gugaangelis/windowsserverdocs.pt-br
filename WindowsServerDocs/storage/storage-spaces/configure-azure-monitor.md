---
title: Entenda e Configure o Azure Monitor
description: Instalação informações detalhadas sobre o que é o Azure Monitor e como configurar alertas de email e sms para seu cluster de espaços de armazenamento diretos no Windows Server 2016 e de 2019.
keywords: Espaços de armazenamento diretos, o azure monitor, notificações, email, sms
ms.assetid: ''
ms.prod: ''
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 3/26/2019
ms.localizationpriority: ''
ms.openlocfilehash: 6b229696e796f176fe89ab250ab48f1d9f0d5666
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280200"
---
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>Use o Monitor do Azure para enviar emails para falhas de serviço de integridade

>Aplica-se a: Windows Server 2019, Windows Server 2016

O Azure Monitor maximiza a disponibilidade e desempenho de seus aplicativos, fornecendo uma solução abrangente para coletar, analisar e atuar em Telemetria da nuvem e ambientes locais. Ele ajuda você a entender como estão o desempenho de seus aplicativos e identifica proativamente os problemas que afetam a eles e os recursos que eles dependem.

Isso é particularmente útil para seu cluster hiperconvergente do local. Com o Azure Monitor integrado, você poderá configurar o email, texto (SMS) e outros alertas para executar ping quando algo está errado com o cluster (ou quando você deseja sinalizar de alguma outra atividade com base nos dados coletados). Abaixo, explicaremos brevemente como funciona o Azure Monitor, como instalar o Azure Monitor e como configurá-lo para enviar notificações.


## <a name="understanding-azure-monitor"></a>Noções básicas sobre o Azure Monitor

Todos os dados coletados pelo Azure Monitor se adapta a um dos dois tipos fundamentais: logs e métricas.

1. [Métricas](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics) são valores numéricos que descrevem alguns aspectos de um sistema em um ponto específico no tempo. Eles são leves e capaz de dar suporte a cenários em tempo real quase. Você verá os dados coletados pelo direito de Azure Monitor em sua página de visão geral no portal do Azure.

![imagem da ingestão de métricas no metrics explorer](media/configure-azure-monitor/metrics.png)

2. [Logs](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#logs) conter diferentes tipos de dados organizados em registros com diferentes conjuntos de propriedades para cada tipo. Telemetria, como eventos e rastreamentos são armazenados como logs além para dados de desempenho, de modo que ele pode todos ser combinado para análise. Dados de logs coletados pelo Azure Monitor podem ser analisados com [consultas](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) rapidamente recuperar, consolidar e analisar os dados coletados. Você pode criar e testar consultas usando [do Log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/portals) no portal do Azure e analisar os dados usando essas ferramentas ou salvar consultas para uso com [visualizações](https://docs.microsoft.com/azure/azure-monitor/visualizations) ou [alerta regras](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

![imagem dos logs de ingestão no log analytics](media/configure-azure-monitor/logs.png)

Teremos mais detalhes abaixo sobre como configurar esses alertas.

## <a name="configuring-health-service"></a>Configuração do serviço de integridade

A primeira coisa que você precisa fazer é configurar seu cluster. Como você deve saber, o [serviço de integridade](../../failover-clustering/health-service-overview.md) melhora a experiência operacional para clusters que executam espaços de armazenamento diretos e monitoramento de rotina. 

Como vimos anteriormente, o Azure Monitor coleta logs de cada nó que ele está em execução no cluster. Portanto, precisamos configurar o serviço de integridade para gravar em um canal de eventos, o que vem a ser:

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

Para configurar o serviço de integridade, execute:

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

Quando você executa o cmdlet acima para definir as configurações de integridade, você fazer com que os eventos que queremos começar que estão sendo gravados para o *Microsoft-Windows-integridade/Operational* canal de evento.

## <a name="configuring-log-analytics"></a>Configuração do Log Analytics

Agora que você configurou o registro em log adequado em seu cluster, a próxima etapa é configurar corretamente o log analytics.

Para fornecer uma visão geral, [Azure Log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows) pode coletar dados diretamente de seus computadores Windows físicos ou virtuais no datacenter ou em outro ambiente de nuvem em um único repositório para correlação e análise detalhadas.

Para entender a configuração com suporte, revise [sistemas operacionais do Windows suportados](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) e [configuração de firewall de rede](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

### <a name="login-in-to-azure-portal"></a>Faça logon no Portal do Azure

Faça logon no portal do Azure em [ https://portal.azure.com ](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

### <a name="create-a-workspace"></a>Criar um espaço de trabalho

Para obter mais detalhes sobre as etapas listadas abaixo, consulte a [documentação do Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-windows-computer).

1. No portal do Azure, clique em **todos os serviços**. Na lista de recursos, digite **do Log Analytics**. Conforme você começa a digitar, a lista filtra com base na sua entrada. Selecione **Log Analytics**.<br><br> 

   ![Portal do Azure](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. Clique em **criar**e, em seguida, selecione opções para os seguintes itens:

   * Forneça um nome para o novo **espaço de trabalho do Log Analytics**, como *DefaultLAWorkspace*. 
   * Selecione uma **assinatura** para vinculá-los selecionando na lista suspensa, se o padrão selecionado não é apropriado.
   * Para **grupo de recursos**, selecione um grupo de recursos que contém um ou mais máquinas virtuais do Azure. <br><br>

      ![Criar folha de recursos do Log Analytics](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. Depois de fornecer as informações necessárias sobre o **espaço de trabalho do Log Analytics** painel, clique em **Okey**.  

Enquanto as informações são verificadas e o espaço de trabalho é criado, você pode acompanhar o progresso em **notificações** no menu. 

### <a name="obtain-workspace-id-and-key"></a>Obter ID do espaço de trabalho e a chave
Antes de instalar o Microsoft monitoramento de agente para Windows, você precisa a ID do espaço de trabalho e a chave do espaço de trabalho do Log Analytics.  Essas informações são necessárias pelo Assistente de instalação para configurar corretamente o agente e garantir que ele possa se comunicar com êxito com o Log Analytics.  

1. No portal do Azure, clique em **todos os serviços** encontrado no canto superior esquerdo. Na lista de recursos, digite **do Log Analytics**. Conforme você começa a digitar, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. Na lista de espaços de trabalho do Log Analytics, selecione *DefaultLAWorkspace* criado anteriormente.
3. Selecione **configurações avançadas**.<br><br> ![Configurações avançadas do log Analytics](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecione **fontes conectadas**e, em seguida, selecione **servidores Windows**.   
5. O valor à direita da vírgula **ID do espaço de trabalho** e **chave primária**. Salvar ambos temporariamente – copie e cole os dois em seu editor favorito por enquanto.   

## <a name="installing-the-agent-on-windows"></a>Instalando o agente no Windows
As etapas a seguir instalar e configurar o Microsoft Monitoring Agent. **Certifique-se de instalar este agente em cada servidor no seu cluster e indicar que você deseja que o agente seja executado na inicialização do Windows.**

1. No **servidores Windows** , selecione apropriado **baixar o agente do Windows** versão para baixar dependendo da arquitetura do processador do sistema operacional Windows.
2. Execute a instalação para instalar o agente em seu computador.
2. Na página **Bem-vindo**, clique em **Avançar**.
3. Sobre o **termos de licença** página, leia a licença e, em seguida, clique em **concordo**.
4. Sobre o **pasta de destino** página, altere ou mantenha a pasta de instalação padrão e, em seguida, clique em **próxima**.
5. Sobre o **opções de instalação do agente** , escolha conectar o agente ao Azure Log Analytics e, em seguida, clique em **próxima**.   
6. Sobre o **Azure Log Analytics** , execute o seguinte:
   1. Cole a **ID do espaço de trabalho** e **chave do espaço de trabalho (chave primária)** que você copiou anteriormente.    
    a. Se o computador precise se comunicar por meio de um servidor proxy para o serviço Log Analytics, clique em **avançado** e forneça a URL e o número da porta do servidor proxy.  Se o servidor proxy exigir autenticação, digite o nome de usuário e a senha para autenticar com o servidor proxy e, em seguida, clique em **próxima**.  
7. Clique em **próxima** depois de ter terminado de fornecer as configurações necessárias.<br><br> ![Cole a ID do espaço de trabalho e a chave primária](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Sobre o **pronto para instalar** página, examine suas escolhas e clique **instalar**.
9. Sobre o **configuração concluída com êxito** , clique em **concluir**.

Ao concluir, o **Microsoft Monitoring Agent** aparece na **painel de controle**. Você pode examinar sua configuração e verificar se o agente está conectado ao Log Analytics. Quando conectado, na **Azure Log Analytics** guia, o agente exibe uma mensagem dizendo: **O Microsoft Monitoring Agent conectou-se com êxito ao serviço Log Analytics do Microsoft.** 

![Status de conexão do MMA ao Log Analytics](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

Para entender a configuração com suporte, revise [sistemas operacionais do Windows suportados](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) e [configuração de firewall de rede](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements).

## <a name="collecting-event-and-performance-data"></a>Coletando dados de desempenho e eventos

Log Analytics pode coletar eventos do log de eventos do Windows e contadores de desempenho que você especificar para relatórios e análise de prazo mais e tomar ação quando determinada condição for detectada.  Siga estas etapas para configurar a coleta de eventos do log de eventos do Windows e vários contadores de desempenho comuns para começar.  

1. No portal do Azure, clique em **mais serviços** encontrado no canto inferior esquerdo. Na lista de recursos, digite **do Log Analytics**. Conforme você começa a digitar, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. Selecione **configurações avançadas**.<br><br> ![Configurações avançadas do log Analytics](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. Selecione **dados**e, em seguida, selecione **Logs de eventos do Windows**.  
4. Aqui, adicione o canal de evento do serviço de integridade digitando o nome abaixo e clique no sinal de adição **+** .  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. Na tabela, marque as severidades **erro** e **aviso**.   
6. Clique em **salvar** na parte superior da página para salvar a configuração.
7. Selecione **contadores de desempenho do Windows** para habilitar a coleta de contadores de desempenho em um computador Windows. 
8. Ao configurar contadores de desempenho do Windows para um novo espaço de trabalho do Log Analytics pela primeira vez, você terá a opção para criar rapidamente vários contadores comuns. Eles são listados com uma caixa de seleção ao lado de cada.<br> ![Contadores de desempenho do Windows padrão selecionados](media/configure-azure-monitor/windows-perfcounters-default.png)<br> Clique em **adicionar os contadores de desempenho selecionados**.  Eles são adicionados e predefinidos com um intervalo de amostragem de coleta de dez segundos.  
9. Clique em **salvar** na parte superior da página para salvar a configuração.

## <a name="creating-alerts-based-on-log-data"></a>Criar alertas com base em dados de log

Se você chegou o momento, seu cluster deve estar enviando seus logs e contadores de desempenho para o Log Analytics. A próxima etapa é criar regras de alerta que executam pesquisas de log automaticamente em intervalos regulares. Se os resultados da pesquisa de logs corresponderem a critérios específicos, um alerta é disparado que envia uma notificação de email ou texto. Vamos explorar isso abaixo.

### <a name="create-a-query"></a>Criar uma consulta

Comece abrindo o portal de pesquisa de Log.   

1. No portal do Azure, clique em **todos os serviços**. Na lista de recursos, digite **Monitor**. Conforme você começa a digitar, a lista filtra com base na sua entrada. Selecione **Monitor**.
2. No menu de navegação do Monitor, selecione **do Log Analytics** e, em seguida, selecione um espaço de trabalho.

A maneira mais rápida de recuperar alguns dados para trabalhar com é uma consulta simples que retorna todos os registros na tabela. Digite as consultas a seguir na caixa de pesquisa e clique no botão de pesquisa.  

```
Event
```

Dados são retornados na exibição de lista padrão e você pode ver o número total de registros foram retornados.

![Consulta simples](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

No lado esquerdo da tela é o painel de filtro que permite adicionar filtragem à consulta sem modificá-la diretamente.  Várias propriedades de registro são exibidas para esse tipo de registro, e você pode selecionar um ou mais valores de propriedade para restringir os resultados da pesquisa.

Marque a caixa de seleção ao lado **erro** sob **EVENTLEVELNAME** ou digite o seguinte para limitar os resultados para eventos de erro.

```
Event | where (EventLevelName == "Error")
```

![Filtro](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

Depois de ter as consultas apropriadas feitas para eventos que importantes para você, salvá-los para a próxima etapa.

### <a name="create-alerts"></a>Criar alertas
Agora, vamos examinar um exemplo para a criação de um alerta.

1. No portal do Azure, clique em **todos os serviços**. Na lista de recursos, digite **do Log Analytics**. Conforme você começa a digitar, a lista filtra com base na sua entrada. Selecione **Log Analytics**.
2. No painel esquerdo, selecione **alertas** e, em seguida, clique em **nova regra de alerta** da parte superior da página para criar um novo alerta.<br><br> ![Criar nova regra de alerta](media/configure-azure-monitor/alert-rule-02.png)<br>
3. Para a primeira etapa, sob o **criar alerta** seção, você vai selecionar seu espaço de trabalho do Log Analytics como o recurso, já que este é um sinal de alerta do log com base.  Filtrar os resultados, escolhendo o específico **assinatura** na lista suspensa se você tiver mais de um, que contém o espaço de trabalho do Log Analytics criado anteriormente.  Filtro de **tipo de recurso** selecionando **Log Analytics** na lista suspensa.  Por fim, selecione o **Resource** **DefaultLAWorkspace** e, em seguida, clique em **feito**.<br><br> ![Criar tarefa de alerta etapa 1](media/configure-azure-monitor/alert-rule-03.png)<br>
4. Na seção **critérios de alerta**, clique em **adicionar critérios** para selecionar sua consulta salva e, em seguida, especifique a lógica que segue a regra de alerta.
5. Configure o alerta com as seguintes informações:  
   a. Dos **com base no** lista suspensa, selecione **medição métrica**.  Uma medida de métrica criará um alerta para cada objeto na consulta com um valor que excede o nosso limite especificado.  
   b. Para o **condição**, selecione **maior** e especifique um thershold.  
   c. Em seguida, defina quando disparar o alerta. Por exemplo, você pode selecionar **violações consecutivas** e, na lista suspensa, selecione **maior** um valor de 3.  
   d. Sob avaliação com base em seção, modifique a **período** valor para **30** minutos e **frequência** para 5. A regra será executada a cada cinco minutos e retornar os registros que foram criados nos últimos trinta minutos da hora atual.  Definir o período de tempo como uma janela maior contas para o potencial de latência de dados e garante que a consulta retorna dados para evitar um falso negativo em que o alerta nunca é acionado.  
6. Clique em **feito** para concluir a regra de alerta.<br><br> ![Configurar o sinal de alerta](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. Agora passar para a segunda etapa, forneça um nome de seu alerta na **nome da regra de alerta** campo, como **alerta em todos os eventos de erro**.  Especifique um **descrição** detalhando as especificações para o alerta e selecione **crítico (Sev 0)** para o **gravidade** valor entre as opções fornecidas.
8. Para ativar imediatamente a regra de alerta na criação, aceite o valor padrão para **Habilitar regra após a criação**.
9. A terceira e última etapa, você deve especificar um **grupo de ação**, que garante que as mesmas ações são executadas cada vez em que um alerta é disparado e pode ser usado para cada regra que você definir. Configure um novo grupo de ação com as seguintes informações:  
   a. Selecione **novo grupo de ação** e o **adicionar grupo de ações** painel será exibido.  
   b. Para **nome do grupo de ação**, especifique um nome, como **operações de TI - notificar** e um **nome curto** como **itops n**.  
   c. Verifique se os valores padrão para **assinatura** e **grupo de recursos** estão corretos. Caso contrário, selecione a correta na lista suspensa.   
   d. Na seção de ações, especifique um nome para a ação, como **Enviar Email** e, em **tipo de ação** selecionar **Email/SMS/Push/voz** na lista suspensa. O **Email/SMS/Push/voz** abrirá o painel de propriedades para a direita a fim de fornecer informações adicionais.  
   e. Sobre o **Email/SMS/Push/voz** painel, selecionar e configurar sua preferência. Por exemplo, habilite **Email** e forneça um endereço de SMTP para entregar a mensagem de email válido.  
   f. Clique em **OK** para salvar suas alterações.<br><br> 

    ![Criar novo grupo de ação](media/configure-azure-monitor/action-group-properties-01.png)

10. Clique em **Okey** para concluir o grupo de ação. 
11. Clique em **criar regra de alerta** para concluir a regra de alerta. Ele começa a ser executado imediatamente.<br><br> ![Concluir a criação de nova regra de alerta](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="test-alerts"></a>Alertas de teste

Para referência, isso é um exemplo de alerta semelhante ao seguinte:

![Exemplo de email de alerta](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>Consulte também

- [Visão geral direta de espaços de armazenamento](storage-spaces-direct-overview.md)
- Para obter mais informações, leia as [documentação do Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata).
