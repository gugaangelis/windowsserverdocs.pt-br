---
title: Entender e configurar Azure Monitor
description: Informações detalhadas de configuração sobre o que Azure Monitor é e como configurar alertas de email e SMS para o cluster de espaços de armazenamento diretos no Windows Server 2016 e 2019.
keywords: Espaços de Armazenamento Diretos, Azure monitor, notificações, email, SMS
ms.assetid: ''
ms.prod: windows-server
ms.author: adagashe
ms.technology: storage-spaces
ms.topic: article
author: adagashe
ms.date: 01/10/2020
ms.openlocfilehash: 878bbce9543439cf78501e496c59e06e077c5ddc
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/26/2020
ms.locfileid: "80308188"
---
# <a name="use-azure-monitor-to-send-emails-for-health-service-faults"></a>Usar Azure Monitor para enviar emails para Serviço de Integridade falhas

>Aplica-se a: Windows Server 2019, Windows Server 2016

O Azure Monitor maximiza a disponibilidade e o desempenho de seus aplicativos, fornecendo uma solução abrangente para coletar, analisar e agir na telemetria de seus ambientes de nuvem e locais. Ele ajuda você a entender como seus aplicativos estão executando e identifica de forma proativa os problemas que os afetam e os recursos dos quais eles dependem.

Isso é particularmente útil para seu cluster hiperconvergente local. Com o Azure Monitor integrado, você poderá configurar email, texto (SMS) e outros alertas para executar o ping quando algo estiver errado com o cluster (ou quando desejar sinalizar alguma outra atividade com base nos dados coletados). Abaixo, explicaremos brevemente como Azure Monitor funciona, como instalar Azure Monitor e como configurá-lo para enviar notificações.

Se você estiver usando o System Center, confira o [pacote de gerenciamento espaços de armazenamento diretos](https://www.microsoft.com/download/details.aspx?id=100782) que monitora os clusters do windows Server 2019 e do windows server 2016 espaços de armazenamento diretos.

Este pacote de gerenciamento inclui:

* Monitoramento de desempenho e integridade do disco físico
* Monitoramento de desempenho e integridade do nó de armazenamento
* Monitoramento de desempenho e integridade do pool de armazenamento
* Tipo de resiliência de volume e status de eliminação de duplicação

## <a name="understanding-azure-monitor"></a>Noções básicas sobre Azure Monitor

Todos os dados coletados pelo Azure Monitor se adaptam a um dos dois tipos fundamentais: métricas e logs.

1. [Métricas](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#metrics) são valores numéricos que descrevem um aspecto de um sistema em um determinado momento. Eles são leves e capazes de dar suporte a cenários quase em tempo real. Você verá os dados coletados pelo Azure Monitor diretamente na página de visão geral na portal do Azure.

![imagem de ingestão de métricas no Metrics Explorer](media/configure-azure-monitor/metrics.png)

2. [Os logs](https://docs.microsoft.com/azure/azure-monitor/platform/data-collection#logs) contêm diferentes tipos de dados organizados em registros com diferentes conjuntos de propriedades para cada tipo. A telemetria, como eventos e rastreamentos, é armazenada como logs além dos dados de desempenho para que todos possam ser combinados para análise. Os dados de log coletados pelo Azure Monitor podem ser analisados com [consultas](https://docs.microsoft.com/azure/azure-monitor/log-query/log-query-overview) para recuperar, consolidar e analisar rapidamente os dados coletados. Você pode criar e testar consultas usando [log Analytics](https://docs.microsoft.com/azure/azure-monitor/log-query/portals) na portal do Azure e, em seguida, analisar diretamente os dados usando essas ferramentas ou salvar consultas para uso com [visualizações](https://docs.microsoft.com/azure/azure-monitor/visualizations) ou [regras de alerta](https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

![imagem de ingestão de logs no log Analytics](media/configure-azure-monitor/logs.png)

Teremos mais detalhes abaixo sobre como configurar esses alertas.

## <a name="onboarding-your-cluster-using-windows-admin-center"></a>Integração do seu cluster usando o centro de administração do Windows

Usando o centro de administração do Windows, você pode integrar seu cluster ao Azure Monitor.

![Gif de cluster de integração para Azure Monitor "](media/configure-azure-monitor/onboarding.gif)

Durante esse fluxo de integração, as etapas abaixo estão acontecendo nos bastidores. Nós detalhamos como configurá-los em detalhes caso você queira configurar manualmente o cluster. 

### <a name="configuring-health-service"></a>Configurando Serviço de Integridade

A primeira coisa que você precisa fazer é configurar o cluster. Como você deve saber, a [serviço de integridade](../../failover-clustering/health-service-overview.md) melhora o monitoramento diário e a experiência operacional dos clusters que executam o espaços de armazenamento diretos. 

Como vimos acima, Azure Monitor coleta logs de cada nó em que ele está sendo executado em seu cluster. Portanto, precisamos configurar o Serviço de Integridade para gravar em um canal de evento, que é:

```
Event Channel: Microsoft-Windows-Health/Operational
Event ID: 8465
```

Para configurar o Serviço de Integridade, execute:

```PowerShell
get-storagesubsystem clus* | Set-StorageHealthSetting -Name "Platform.ETW.MasTypes" -Value "Microsoft.Health.EntityType.Subsystem,Microsoft.Health.EntityType.Server,Microsoft.Health.EntityType.PhysicalDisk,Microsoft.Health.EntityType.StoragePool,Microsoft.Health.EntityType.Volume,Microsoft.Health.EntityType.Cluster"
```

Ao executar o cmdlet acima para definir as configurações de integridade, você faz com que os eventos que desejamos comecem a ser gravados no canal de eventos *Microsoft-Windows-Health/Operational* .

### <a name="configuring-log-analytics"></a>Configurando Log Analytics

Agora que você configurou o registro em log apropriado no cluster, a próxima etapa é configurar corretamente o log Analytics.

Para fornecer uma visão geral, o [Azure log Analytics](https://docs.microsoft.com/azure/azure-monitor/platform/agent-windows) pode coletar dados diretamente de computadores físicos ou virtuais do Windows em seu datacenter ou em outro ambiente de nuvem em um único repositório para análise e correlação detalhadas.

Para entender a configuração com suporte, examine os [sistemas operacionais Windows](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) e a [configuração de firewall de rede](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements)com suporte.

Se você não tiver uma assinatura do Azure, crie uma [conta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de começar.

#### <a name="login-in-to-azure-portal"></a>Fazer logon no portal do Azure

Faça logon no portal do Azure em [https://portal.azure.com](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

#### <a name="create-a-workspace"></a>Criar um workspace

Para obter mais detalhes sobre as etapas listadas abaixo, consulte a [documentação do Azure monitor](https://docs.microsoft.com/azure/azure-monitor/learn/quick-collect-windows-computer).

1. Na portal do Azure, clique em **todos os serviços**. Na lista de recursos, digite **log Analytics**. Conforme você começa a digitar, a lista filtra com base em sua entrada. Selecione **log Analytics**.<br><br> 

   ![Portal do Azure](media/configure-azure-monitor/azure-portal-01.png)<br><br>

2. Clique em **criar**e selecione opções para os seguintes itens:

   * Forneça um nome para o novo **espaço de trabalho log Analytics**, como *DefaultLAWorkspace*. 
   * Selecione uma **assinatura** à qual vincular selecionando na lista suspensa se o padrão selecionado não for apropriado.
   * Para **grupo de recursos**, selecione um grupo de recursos existente que contenha uma ou mais máquinas virtuais do Azure. <br><br>

      ![Criar Log Analytics folha de recursos](media/configure-azure-monitor/create-loganalytics-workspace-02.png) <br><br>  

3. Depois de fornecer as informações necessárias no painel **log Analytics espaço de trabalho** , clique em **OK**.  

Embora as informações sejam verificadas e o espaço de trabalho seja criado, você pode acompanhar seu progresso em **notificações** no menu. 

#### <a name="obtain-workspace-id-and-key"></a>Obter ID e chave do espaço de trabalho
Antes de instalar o Microsoft Monitoring Agent para Windows, você precisa da ID e da chave do espaço de trabalho para seu espaço de trabalho do Log Analytics.  Essas informações são exigidas pelo assistente de instalação para configurar corretamente o agente e garantir que ele possa se comunicar com êxito com Log Analytics.  

1. Na portal do Azure, clique em **todos os serviços** encontrados no canto superior esquerdo. Na lista de recursos, digite **log Analytics**. Conforme você começa a digitar, a lista filtra com base em sua entrada. Selecione **log Analytics**.
2. Na lista de espaços de trabalho do Log Analytics, selecione *DefaultLAWorkspace* criado anteriormente.
3. Selecione **Configurações avançadas**.<br><br> ![Log Analytics configurações avançadas](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br>  
4. Selecione **fontes conectadas**e, em seguida, selecione **servidores Windows**.   
5. O valor à direita da **ID do espaço de trabalho** e da **chave primária**. Salve temporariamente as cópias e cole-as em seu editor favorito por enquanto.   

### <a name="installing-the-agent-on-windows"></a>Instalando o agente no Windows
As etapas a seguir instalam e configuram o Microsoft Monitoring Agent. **Certifique-se de instalar esse agente em cada servidor no cluster e indicar que você deseja que o agente seja executado na inicialização do Windows.**

1. Na página **servidores Windows** , selecione a versão de **Download** apropriada do agente do Windows para download, dependendo da arquitetura do processador do sistema operacional Windows.
2. Execute a instalação do para instalar o agente em seu computador.
2. Na página **Boas-vindas**, clique em **Avançar**.
3. Na página **termos de licença** , leia a licença e clique em **concordo**.
4. Na página **pasta de destino** , altere ou mantenha a pasta de instalação padrão e clique em **Avançar**.
5. Na página **Opções de instalação do agente** , escolha conectar o agente ao log Analytics do Azure e clique em **Avançar**.   
6. Na página **log Analytics do Azure** , faça o seguinte:
   1. Cole a **ID do espaço de trabalho** e a **chave do espaço de trabalho (chave primária)** que você copiou anteriormente.    
    a. Se o computador precisar se comunicar por meio de um servidor proxy para o serviço de Log Analytics, clique em **avançado** e forneça a URL e o número da porta do servidor proxy.  Se o servidor proxy exigir autenticação, digite o nome de usuário e a senha para autenticar com o servidor proxy e clique em **Avançar**.  
7. Clique em **Avançar** depois de concluir o fornecimento das definições de configuração necessárias.<br><br> ![colar a ID do espaço de trabalho e a chave primária](media/configure-azure-monitor/log-analytics-mma-setup-laworkspace.png)<br><br>
8. Na página **pronto para instalar** , examine suas escolhas e clique em **instalar**.
9. Na página **configuração concluída com êxito** , clique em **concluir**.

Ao concluir, o **Microsoft Monitoring Agent** aparece no **painel de controle**. Você pode examinar sua configuração e verificar se o agente está conectado a Log Analytics. Quando conectado, na guia **log Analytics do Azure** , o agente exibe uma mensagem dizendo: **a Microsoft Monitoring Agent se conectou com êxito ao serviço Microsoft log Analytics.** 

![Status de conexão do MMA para Log Analytics](media/configure-azure-monitor/log-analytics-mma-laworkspace-status.png)

Para entender a configuração com suporte, examine os [sistemas operacionais Windows](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#supported-windows-operating-systems) e a [configuração de firewall de rede](https://docs.microsoft.com/azure/azure-monitor/platform/log-analytics-agent#network-firewall-requirements)com suporte.

## <a name="setting-up-alerts-using-windows-admin-center"></a>Configurando alertas usando o centro de administração do Windows

No centro de administração do Windows, você pode configurar alertas padrão que serão aplicados a todos os servidores em seu espaço de trabalho Log Analytics. 

![Gif de configuração de alertas "](media/configure-azure-monitor/setup1.gif)

Estes são os alertas e suas condições padrão que você pode aceitar:

| Nome do alerta                | Condição padrão                                  |
|---------------------------|----------------------------------------------------|
| Utilização da CPU           | Mais de 85% por 10 minutos                            |
| Utilização da capacidade do disco | Mais de 85% por 10 minutos                            |
| Utilização de memória        | Memória disponível com menos de 100 MB por 10 minutos   |
| Pulsação                 | Menos de 2 batidas por 5 minutos                   |
| Erro crítico do sistema     | Qualquer alerta crítico no log de eventos do sistema de cluster |
| Alerta do serviço de integridade      | Qualquer falha no serviço de integridade no cluster            |

Depois de configurar os alertas no centro de administração do Windows, você poderá ver os alertas em seu espaço de trabalho do log Analytics no Azure.

![Gif de configuração de alertas "](media/configure-azure-monitor/setup2.gif)

Durante esse fluxo de integração, as etapas abaixo estão acontecendo nos bastidores. Nós detalhamos como configurá-los em detalhes caso você queira configurar manualmente o cluster. 

### <a name="collecting-event-and-performance-data"></a>Coletando dados de eventos e de desempenho

Log Analytics pode coletar eventos do log de eventos do Windows e dos contadores de desempenho que você especificar para análise e relatórios de longo prazo e tomar medidas quando uma determinada condição for detectada.  Siga estas etapas para configurar a coleta de eventos do log de eventos do Windows e vários contadores de desempenho comuns para começar.  

1. Na portal do Azure, clique em **mais serviços** encontrado no canto inferior esquerdo. Na lista de recursos, digite **log Analytics**. Conforme você começa a digitar, a lista filtra com base em sua entrada. Selecione **log Analytics**.
2. Selecione **Configurações avançadas**.<br><br> ![Log Analytics configurações avançadas](media/configure-azure-monitor/log-analytics-advanced-settings-01.png)<br><br> 
3. Selecione **dados**e, em seguida, selecione **logs de eventos do Windows**.  
4. Aqui, adicione o canal de evento Serviço de Integridade digitando o nome abaixo e clique no sinal de mais **+** .  
   ```
   Event Channel: Microsoft-Windows-Health/Operational
   ```
5. Na tabela, verifique o **erro** de severidades e o **aviso**.   
6. Clique em **salvar** na parte superior da página para salvar a configuração.
7. Selecione **contadores de desempenho do Windows** para habilitar a coleta de contadores de desempenho em um computador Windows. 
8. Ao configurar os contadores de desempenho do Windows pela primeira vez para um novo espaço de trabalho Log Analytics, você terá a opção de criar rapidamente vários contadores comuns. Eles são listados com uma caixa de seleção ao lado de cada um.<br> ![contadores de desempenho do Windows padrão selecionados](media/configure-azure-monitor/windows-perfcounters-default.png)<br> Clique em **adicionar os contadores de desempenho selecionados**.  Eles são adicionados e predefinidos com um intervalo de exemplo de coleta de dez segundos.  
9. Clique em **salvar** na parte superior da página para salvar a configuração.

## <a name="creating-alerts-based-on-log-data"></a>Criando alertas com base em dados de log

Se você o fez até agora, o cluster deve enviar seus logs e contadores de desempenho para Log Analytics. A próxima etapa é criar regras de alerta que executem pesquisas de log automaticamente em intervalos regulares. Se os resultados da pesquisa de log corresponderem a critérios específicos, um alerta será disparado que enviará um email ou notificação de texto. Vamos explorar isso abaixo.

### <a name="create-a-query"></a>Criar uma consulta

Comece abrindo o portal de pesquisa de logs.   

1. Na portal do Azure, clique em **todos os serviços**. Na lista de recursos, digite **Monitor**. Conforme você começa a digitar, a lista filtra com base em sua entrada. Selecione **Monitor**.
2. No menu de navegação do monitor, selecione **log Analytics** e, em seguida, selecione um espaço de trabalho.

A maneira mais rápida de recuperar alguns dados para trabalhar é uma consulta simples que retorna todos os registros na tabela. Digite as consultas a seguir na caixa de pesquisa e clique no botão Pesquisar.  

```
Event
```

Os dados são retornados na exibição de lista padrão e você pode ver quantos registros totais foram retornados.

![Consulta simples](media/configure-azure-monitor/log-analytics-portal-eventlist-01.png)

No lado esquerdo da tela está o painel de filtro que permite adicionar filtragem à consulta sem modificá-la diretamente.  Várias propriedades de registro são exibidas para esse tipo de registro e você pode selecionar um ou mais valores de propriedade para restringir os resultados da pesquisa.

Marque a caixa de seleção ao lado de **erro** em **EVENTLEVELNAME** ou digite o seguinte para limitar os resultados a eventos de erro.

```
Event | where (EventLevelName == "Error")
```

![Filtro](media/configure-azure-monitor/log-analytics-portal-eventlist-02.png)

Depois de ter as consultas do approriate feitas para os eventos com os quais você se preocupa, salve-as na próxima etapa.

### <a name="create-alerts"></a>Criar alertas
Agora, vamos examinar um exemplo para criar um alerta.

1. Na portal do Azure, clique em **todos os serviços**. Na lista de recursos, digite **log Analytics**. Conforme você começa a digitar, a lista filtra com base em sua entrada. Selecione **log Analytics**.
2. No painel esquerdo, selecione **alertas** e clique em **nova regra de alerta** na parte superior da página para criar um novo alerta.<br><br> ![criar nova regra de alerta](media/configure-azure-monitor/alert-rule-02.png)<br>
3. Para a primeira etapa, na seção **criar alerta** , você vai selecionar seu espaço de trabalho log Analytics como o recurso, pois esse é um sinal de alerta baseado em log.  Filtre os resultados escolhendo a **assinatura** específica na lista suspensa se você tiver mais de um, que contém log Analytics espaço de trabalho criado anteriormente.  Filtre o **tipo de recurso** selecionando **log Analytics** na lista suspensa.  Por fim, selecione o **recurso** **DefaultLAWorkspace** e clique em **concluído**.<br><br> ![criar a tarefa etapa 1 do alerta](media/configure-azure-monitor/alert-rule-03.png)<br>
4. Na seção **critérios de alerta**, clique em **Adicionar critérios** para selecionar a consulta salva e, em seguida, especifique a lógica que a regra de alerta segue.
5. Configure o alerta com as seguintes informações:  
   a. Na lista suspensa **com base em** , selecione medição de **métrica**.  Uma medida métrica criará um alerta para cada objeto na consulta com um valor que exceda nosso limite especificado.  
   b. Para a **condição**, selecione **maior que** e especifique um thershold.  
   c. Em seguida, defina quando disparar o alerta. Por exemplo, você pode selecionar **violações consecutivas** e, na lista suspensa, selecionar **maior que** um valor de 3.  
   d. Em avaliação baseada na seção, modifique o valor do **período** para **30** minutos e **frequência** como 5. A regra será executada a cada cinco minutos e retornará os registros que foram criados nos últimos trinta minutos a partir da hora atual.  Definir o período de tempo para uma conta de janela mais ampla para o potencial de latência de dados e garante que a consulta retorne dados para evitar um falso negativo em que o alerta nunca é acionado.  
6. Clique em **concluído** para concluir a regra de alerta.<br><br> ![configurar o sinal de alerta](media/configure-azure-monitor/alert-signal-logic-02.png)<br> 
7. Agora, passando para a segunda etapa, forneça um nome de seu alerta no campo **nome da regra de alerta** , como **alertar sobre todos os eventos de erro**.  Especifique uma **Descrição** detalhando as especificações do alerta e selecione **crítico (Sev 0)** para o valor de **severidade** das opções fornecidas.
8. Para ativar imediatamente a regra de alerta na criação, aceite o valor padrão para **habilitar a regra na criação**.
9. Para a terceira e última etapa, você especifica um **grupo de ações**, que garante que as mesmas ações sejam executadas sempre que um alerta for disparado e possa ser usado para cada regra que você definir. Configure um novo grupo de ação com as seguintes informações:  
   a. Selecione **novo grupo de ações** e o painel **Adicionar grupo de ações** é exibido.  
   b. Para **nome do grupo de ações**, especifique um nome como **operações de ti-notificar** e um **nome curto** , como **itops-n**.  
   c. Verifique se os valores padrão para **assinatura** e **grupo de recursos** estão corretos. Caso contrário, selecione o correto na lista suspensa.   
   d. Na seção ações, especifique um nome para a ação, como **Enviar email** e, em **tipo de ação** , selecione **email/SMS/Push/voz** na lista suspensa. O painel de propriedades **email/SMS/push/de voz** será aberto à direita para fornecer informações adicionais.  
   e. No painel **email/SMS/Push/voz** , selecione e configure sua preferência. Por exemplo, habilite o **email** e forneça um endereço SMTP de email válido para entregar a mensagem.  
   f. Clique em **OK** para salvar suas alterações.<br><br> 

    ![Criar novo grupo de ação](media/configure-azure-monitor/action-group-properties-01.png)

10. Clique em **OK** para concluir o grupo de ações. 
11. Clique em **criar regra de alerta** para concluir a regra de alerta. Ele começa a ser executado imediatamente.<br><br> ![concluir a criação de uma nova regra de alerta](media/configure-azure-monitor/alert-rule-01.png)<br> 

### <a name="example-alert"></a>Alerta de exemplo

Para referência, é assim que um alerta de exemplo se parece no Azure.

![Gif de alerta no Azure](media/configure-azure-monitor/alert.gif)

Abaixo está um exemplo do email que será enviado por Azure Monitor:

![Exemplo de email de alerta](media/configure-azure-monitor/warning.png)

## <a name="see-also"></a>Consulte também

- [Visão geral de Espaços de Armazenamento Diretos](storage-spaces-direct-overview.md)
- Para obter informações mais detalhadas, leia a [documentação do Azure monitor](https://docs.microsoft.com/azure/azure-monitor/learn/tutorial-viewdata).
- Leia este para obter uma visão geral de como [se conectar a outros serviços híbridos do Azure](../../manage/windows-admin-center/azure/index.md).
