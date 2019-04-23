---
title: Exibir e configurar o evento de desempenho e dados de serviço
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: ccd59c35-4dbf-48e7-88a4-c519c00184d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 727b81d1ba2cda32a7568e4e2f23065b1589e745
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861897"
---
# <a name="view-and-configure-performance-event-and-service-data"></a>Exibir e configurar dados de desempenho, eventos e serviços

>Aplica-se a: Windows Server (canal semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como exibir e configurar as entradas de log de eventos, contadores de desempenho e alertas de serviço que são exibidos para servidores locais e remotos no Gerenciador do servidor.  

Dados de log de eventos, serviços e desempenho são exibidos em dois locais no console do Gerenciador do servidor no Windows Server.  

-   No painel, você pode clicar o **eventos**, **desempenho**, e **serviços** linhas das miniaturas para configurar eventos, desempenho e dados de log de serviço que você deseja ver para funções, todo o pool de servidor do Gerenciador de servidores, grupos de servidores criados pelo usuário e o servidor local. Clicar no hipertexto linhas **modo de exibição de detalhe** caixas de diálogo que lhe permitem especificam os dados sobre o qual você deseja ser alertado no dashboard. Depois de configurar eventos, serviços e dados de log de desempenho que deverão ser realçados nas miniaturas do dashboard, entradas de log que correspondem aos critérios especificados são listadas na parte inferior a **modo de exibição de detalhe** caixas de diálogo.  

-   Os blocos **Eventos**, **Serviços** e **Desempenho** fazem parte das home pages de funções e grupos. Os comandos no menu **Tarefas** desses blocos permitem especificar os dados que serão coletados dos servidores gerenciados. Os blocos incluem filtros e consultas para limitar ainda mais as entradas de log exibidas no bloco, se desejado.  

Este tópico contém as seguintes seções.  

-   [O que são miniaturas?](#BKMK_thumb)  

-   [Exibir e configurar eventos](#BKMK_events)  

-   [Exibir e configurar contadores de desempenho](#BKMK_perf)  

-   [Gerenciar serviços e configurar alertas de serviço](#BKMK_services)  

-   [Exibir e copiar entradas de eventos ou desempenho](#BKMK_copy)  

## <a name="BKMK_thumb"></a>O que são miniaturas?  
*Miniaturas* são exibidos no painel do Gerenciador do servidor para cada função (miniatura de uma função reflete os dados coletados sobre todos os servidores no pool de Gerenciador de servidores que executam a função), para cada grupo de servidores, para o **todos os Servidores** grupo (todos os servidores no pool do Gerenciador do servidor) e para o servidor local. Depois que o Gerenciador do servidor recebe os dados dos servidores gerenciados, as miniaturas são automaticamente criadas para as funções que são executados em servidores no pool de servidores.  

Se o console do Gerenciador do servidor estiver em execução em um computador cliente como parte das ferramentas de administração de servidor remoto, não há nenhuma **servidor Local** miniatura.  

A miniatura faz uma exibição rápida do status e da capacidade de gerenciamento de funções, servidores e grupos de servidores. A linha de título da miniatura muda de cor (e os números realçados são exibidos na margem esquerda) quando eventos, contadores de desempenho, resultados de analisador de práticas recomendadas, serviços ou problemas gerais de gerenciamento atendem aos critérios que você configurar o **modo de exibição de detalhe** caixas de diálogo abertas quando você clica nas linhas das miniaturas. A tabela a seguir descreve os dados exibidos nas miniaturas.  

|Linha de miniaturas|Descrição|  
|---------|--------|  
|Capacidade de gerenciamento|A capacidade de gerenciamento de um servidor inclui várias medidas: se o servidor está online ou offline, seja ao Gerenciador do servidor, de dados acessíveis e emissão de relatórios se o usuário que fez logon no computador local tem direitos de usuário adequados para acessar ou gerenciar o servidor remoto, se o servidor remoto está executando todo o software necessário para gerenciar remotamente, ou se o servidor está configurado de forma que permite que ele seja consultado e gerenciado usando o Gerenciador do servidor. Os dados de capacidade de gerenciamento único Gerenciador do servidor pode coletar de um servidor que está executando o Windows Server 2003 são se o servidor está online ou offline. Para obter informações detalhadas sobre erros de status de gerenciabilidade e como resolvê-los, consulte o [Guia de solução de problemas do Gerenciador do Servidor](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).|  
|Eventos|Você pode configurar a linha de **Eventos** de uma miniatura para exibir alertas quando os eventos registrados correspondem aos níveis de severidade, às fontes, aos períodos de tempo, servidores ou IDs de eventos que você especificar. Exibir detalhes sobre os eventos e altere os alertas que você deseja ver clicando o **eventos** linha e abrir o **exibição de detalhes de eventos** caixa de diálogo para o grupo de funções ou servidores.|  
|Serviços|Você pode configurar o **Services** linha para exibir alertas quando os serviços são localizados em um grupo de funções ou servidores que correspondem aos tipos de inicialização, status do serviço, nomes de serviço e servidores que você especifica o **modo de exibição de detalhes de serviços**  caixa de diálogo.<br /><br />Depois que um servidor foi adicionado ao pool de servidores do Gerenciador do servidor, os alertas de serviço sobre o serviço de detecção do Hardware do Shell podem ser exibidas se não houver nenhum usuário conectado ao servidor gerenciado. Isso ocorre porque o serviço de detecção do hardware do shell é executado apenas quando usuários estão conectados ao servidor gerenciado ou a uma sessão de área de trabalho remota no servidor gerenciado. Para impedir a exibição de alertas do serviço de detecção do hardware do shell nesse caso, clique em **Serviços** nas miniaturas dos grupos de servidores, incluindo o grupo **Todos os servidores**. No **serviços de modo de exibição de detalhe** caixa de diálogo a **serviços** lista suspensa, desmarque a caixa de seleção **detecção do Hardware do Shell**e, em seguida, clique em **Okey**.|  
|Desempenho|Você pode configurar o **desempenho** linha para exibir alertas para um grupo de funções ou servidores quando ocorrerem alertas de desempenho que correspondem aos tipos de recursos, servidores ou períodos de tempo que você especificar na **deexibiçãodedetalhesdedesempenho** caixa de diálogo.<br /><br />Por padrão, os contadores de desempenho estão desativados. Servidores gerenciados que executam sistemas operacionais mais recentes que o Windows Server 2003 e de desempenho contadores não tiverem sido iniciados, costumam mostram gerenciamento erros de status **on-line - contadores de desempenho não iniciado** no **servidores** lado a lado das páginas de função ou grupo. Para ativar os contadores de desempenho em servidores gerenciados, na **todos os servidores** página, clique com botão direito entradas na **desempenho** lado a lado que mostram uma **Status do contador** valor de  **Desativar**e, em seguida, clique em **iniciar contadores de desempenho**. Você também pode iniciar contadores de desempenho clicando nas entradas de servidores na **servidores** lado a lado da função ou páginas de grupo e, em seguida, clicando em **iniciar contadores de desempenho**.|  
|Resultados BPA|Você pode configurar o **resultados BPA** linha para exibir alertas para um função ou grupo de servidores quando os resultados da varredura BPA estão localizados que correspondem aos níveis de severidade, servidores ou às categorias BPA que você especificar na **deexibiçãodedetalhederesultadosdoBPA** caixa de diálogo.|  

## <a name="BKMK_events"></a>Exibir e configurar eventos  
Nesta seção, Aprenda a configurar quais dados de log de eventos são coletados dos servidores no pool de servidores do Gerenciador do servidor e quais eventos você deseja realçar nas miniaturas.  

> [!NOTE]  
> Os eventos que você deseja receber alertas nas miniaturas são um subconjunto do total de eventos que instruem o Gerenciador do servidor para coletar dos servidores gerenciados. Embora a alteração dos critérios de evento na **configurar dados do evento** da caixa de diálogo **eventos** blocos podem alterar os números de alertas mostrados no painel do Gerenciador do servidor, alterando os critérios de alerta de evento em miniaturas não tem nenhum efeito nos dados de log de eventos que são coletados dos servidores gerenciados.  

#### <a name="to-configure-the-events-collected-from-managed-servers"></a>Para configurar os eventos coletados dos servidores gerenciados  

1.  No console do Gerenciador do servidor, abra qualquer página, exceto do dashboard. É possível configurar os eventos que deseja coletar dos servidores gerenciados no bloco **Eventos** nas páginas da função, do grupo de servidores ou do servidor local.  

2.  No menu **Tarefas** do bloco **Eventos** , clique em **Configurar Dados do Evento**.  

3.  Selecione os níveis de severidade do evento que você deseja coletar dos servidores no grupo selecionado. Por padrão, os níveis de severidade **Crítico**, **Erro** e **Aviso** estão selecionados.  

4.  Especifique um período de tempo durante o qual ocorrem os eventos. A duração padrão de eventos é 24 horas.  

5.  Selecione os arquivos de log de eventos do qual você deseja que eventos sejam coletados. Os padrões são **Aplicativo**, **Configuração**e **Sistema**.  

6.  Para salvar as alterações, clique em **OK** para fechar a caixa de diálogo **Configurar Dados do Evento** . Os dados do evento são atualizados automaticamente quando as alterações são salvas.  

#### <a name="to-configure-the-events-highlighted-in-thumbnails"></a>Para configurar os eventos destacados em miniaturas  

1.  Se o Gerenciador do servidor já estiver aberto, vá para a próxima etapa. Se o Gerenciador do Servidor ainda não estiver aberto, abra-o de uma das maneiras a seguir.  

    -   Na área de trabalho do Windows, inicie o Gerenciador do Servidor clicando em **Gerenciador do Servidor** na barra de tarefas do Windows.  

    -   Na tela **inicial** do Windows, clique no bloco do Gerenciador de servidores.  

2.  Na página do dashboard, em uma miniatura do bloco **Grupos de Funções e Servidores** , clique na linha **Eventos** .  

3.  No **modo de exibição de detalhes de eventos** caixa de diálogo caixa, adicione um nível de severidade aos eventos que você deseja exibir. Por padrão, as miniaturas só exibem o realce de alerta para eventos críticos. Observe que o número de eventos exibido na **modo de exibição de detalhe** caixa de diálogo aumenta quando você adiciona um nível de severidade, sobre o qual você deseja ser alertado.  

4.  No campo **Fontes de evento**, selecione as fontes de evento para as quais deseja receber alerta. O padrão é **All**.  

5.  Se a miniatura for referente uma função que é instalada em vários servidores ou um grupo de vários servidores, você pode selecionar os servidores para os quais deseja receber alertas de eventos na **servidores** lista suspensa.  

6.  No **período de tempo** campo, especifique um período de tempo até 1440 minutos, 24 horas ou 1 dia.  

7.  No campo **IDs de Eventos**, digite os números das IDs dos eventos específicos para os quais deseja receber alerta. Você pode digitar um intervalo de IDs de eventos separadas por um traço (**-**) e excluir IDs de eventos do intervalo digitando o traço antes da ID do evento ou do intervalo de IDs de eventos que deseja excluir. Por exemplo, o valor **1,3,5-99,-76** significa que os alertas são acionados para as IDs de eventos 1 e 3 e para todos os eventos com IDs entre 5 e 99, exceto para a ID de evento 76.  

8.  À medida que você altera os critérios para os quais os alertas são exibidos, o número de alertas de eventos que é exibido no painel de resultados na parte interior da caixa de diálogo pode ser alterado. Selecione as entradas na lista e clique em **ocultar alertas** para impedir que eles afetem a contagem de alertas é exibida na miniatura de origem. Pressione e segure **Ctrl** enquanto seleciona os alertas para selecionar vários ao mesmo tempo. Você pode fazer isso para os alertas que correspondem aos seus critérios de alerta de eventos, mas que você não precisa ver.  

9. Clique em **Mostrar Tudo** para retornar os alertas ocultos à lista.  

10. Clique em **Okey** para salvar suas alterações, feche o **exibição de detalhe** caixa de diálogo e exibir alterações do alerta de eventos na miniatura de origem.  

## <a name="BKMK_perf"></a>Exibir e configurar dados de log de desempenho  
Nesta seção, Aprenda a configurar quais dados de log de desempenho são coletados dos servidores no pool de servidores do Gerenciador do servidor e qual contador de desempenho de alertas você deseja realçar nas miniaturas.  

Por padrão, os contadores de desempenho estão desativados. Servidores gerenciados que executam sistemas operacionais mais recentes que o Windows Server 2003 e de desempenho contadores não tiverem sido iniciados, costumam mostram gerenciamento erros de status **on-line - contadores de desempenho não iniciado** no **servidores** lado a lado das páginas de função ou grupo. Para ativar os contadores de desempenho em servidores gerenciados, na **todos os servidores** página, clique com botão direito entradas na **desempenho** lado a lado que mostram uma **Status do contador** valor de  **Desativar**e, em seguida, clique em **iniciar contadores de desempenho**. Você também pode iniciar contadores de desempenho clicando nas entradas de servidores na **servidores** lado a lado da função ou páginas de grupo e, em seguida, clicando em **iniciar contadores de desempenho**.  

> [!NOTE]  
> Os alertas de desempenho exibidos nas miniaturas são um subconjunto dos dados de contador de desempenho total que instruem o Gerenciador do servidor para coletar dos servidores gerenciados. Embora a alteração de critérios de alerta de desempenho na **configurar alertas de desempenho** da caixa de diálogo **desempenho** blocos podem alterar os números de alertas mostrados no painel do Gerenciador do servidor, alterando os critérios de alerta de desempenho nas miniaturas não tem nenhum efeito sobre os dados de log de desempenho são coletados dos servidores gerenciados.  
>   
> Por isso, a duração máxima dos dados de desempenho que você pode exibir nas miniaturas não pode ser maior que o período de exibição de gráfico máximo configurado na caixa de diálogo **Configurar alertas de desempenho** . Por exemplo, se o **período de exibição de gráfico** valor em **configurar alertas de desempenho** está **1 dia**, o valor máximo para o **período**campo em uma **modo de exibição de detalhes de desempenho** caixa de diálogo que você abriu no painel Gerenciador do servidor pode ser **1 dia**, **24 horas**, ou **1.440 minutos**.  

#### <a name="to-configure-the-performance-log-data-collected-from-managed-servers"></a>Para configurar os dados do log de desempenho coletados dos servidores gerenciados  

1.  No console do Gerenciador do servidor, abra qualquer página, exceto do dashboard. É possível configurar os dados de desempenho que deseja coletar dos servidores gerenciados no bloco **Desempenho** nas páginas da função, do grupo de servidores ou do servidor local.  

2.  Para coletar os dados do log de desempenho dos servidores gerenciados, os contadores de desempenho devem estar ligados. Se os contadores de desempenho estiverem desligados, clique com botão direito uma entrada na **desempenho** lista de blocos e, em seguida, clique em **iniciar contadores de desempenho**. A coleta de dados do contador de desempenho pode levar algum tempo, dependendo do número de servidores dos quais os dados são coletados e da largura de banda de rede disponível. Exiba o status na coluna **Status do Contador** .  

3.  No menu **Tarefas** do bloco **Desempenho** , clique em **Configurar Alertas de Desempenho**.  

4.  para os servidores no grupo selecionado, ou que estão executando a função selecionada, especifique o percentual de uso da CPU em alertas do contador de desempenho coletadas pelo Gerenciador do servidor. O padrão é 85%.  

5.  Especifique a memória restante disponível, em megabytes, que os servidores devem ter antes que os alertas do contador de desempenho possam ser coletados. O valor padrão é 2 MB.  

6.  Especifique o período de tempo a ser exibido nos gráficos para os recursos **Uso de CPU** e **Memória Disponível** no bloco **Desempenho** na página selecionada. O período padrão é de um dia. Clique em **Salvar**.  

    Observe que o número de alertas de desempenho no bloco **Desempenho** e o mapeamento dos alertas ao longo do tempo conforme exibido pelo gráfico podem mudar depois que você clica em **Salvar**.  

    > [!NOTE]  
    > para máquinas virtuais que tenham [memória dinâmica](https://technet.microsoft.com/library/ff817651.aspx) ativada, aumento no limite de alertas de desempenho pode resultar em alertas de falso positivo.  

7.  Para atualizar a lista de alertas de desempenho coletados dos servidores, no menu **Tarefas**, clique em **Atualizar**.  

#### <a name="to-configure-the-performance-alerts-highlighted-in-thumbnails"></a>Para configurar os alertas de desempenho destacados em miniaturas  

1.  Na página do dashboard, em uma miniatura do bloco **Grupos de Funções e Servidores** , clique na linha **Desempenho** .  

2.  No **modo de exibição de detalhes de desempenho** caixa de diálogo, marcar ou desmarcar as caixas de seleção para os limites de desempenho de recursos sobre os quais você deseja ser alertado na **tipo de recurso** campo. Observe que o número de alertas de desempenho exibido na **modo de exibição de detalhe** caixa de diálogo pode aumentar quando você adiciona um limite de desempenho do recurso sobre o qual você deseja ser alertado.  

3.  Se a miniatura for referente uma função que é instalada em vários servidores ou um grupo de vários servidores, você pode selecionar os servidores para os quais deseja receber alertas de desempenho na **servidores** lista suspensa.  

4.  No **período de tempo** campo, especifique um período de tempo até 1440 minutos, 24 horas ou 1 dia.  

5.  À medida que você altera os critérios para os quais os alertas são exibidos, o número de alertas que é exibido no painel de resultados na parte interior da caixa de diálogo pode ser alterado. Clique em **Ocultar Alertas** para ocultar todos os alertas mais antigos que o período atual e evitar que eles afetem a contagem de alertas exibida na miniatura de origem.  

6.  Clique em **Mostrar Tudo** para retornar os alertas ocultos à lista.  

7.  Clique em **Okey** para salvar suas alterações, feche o **exibição de detalhe** caixa de diálogo e exibir alterações do alerta de desempenho na miniatura de origem.  

#### <a name="to-view-the-properties-of-performance-alerts"></a>Para exibir as propriedades de alertas de desempenho  

1.  Siga um destes procedimentos.  

    -   Na página do dashboard, em uma miniatura do bloco **Grupos de Funções e Servidores** , clique na linha **Desempenho** .  

    -   Abra a home page da função ou do grupo e localize o bloco **Desempenho** referente à função ou ao grupo.  

2.  Clique duas vezes em um alerta de desempenho na lista para exibir suas propriedades. Se preferir, clique com o botão direito do mouse em um alerta de desempenho e clique em **Exibir Propriedades**.  

3.  Na caixa de diálogo **Propriedades de Alertas de Desempenho**, selecione as entradas de log para exibir as informações sobre os processos associados à entrada na área **Processos**.  

4.  Quando você tiver terminado de exibir as propriedades do alerta de desempenho, feche a caixa de diálogo.  

### <a name="analyze-performance-data-and-solve-problems"></a>Analisar dados de desempenho e resolver problemas  
Para obter mais informações sobre como analisar dados do contador de desempenho que podem ser exibidos no Gerenciador do servidor e solucionar problemas de desempenho em servidores gerenciados, consulte os seguintes recursos.  

-   [Analisando dados de desempenho](https://go.microsoft.com/fwlink/?LinkId=239829)  

-   [Solucionar problemas de desempenho](https://go.microsoft.com/fwlink/?LinkId=239831)  

Para obter mais informações sobre as ferramentas de análise e monitoramento que estão disponíveis para o Windows Server 2012 de desempenho avançado e versões posteriores do Windows Server, incluindo o Server Performance Advisor 3.0, consulte [desempenho](https://msdn.microsoft.com/windows/hardware/gg463374.aspx) no MSDN.  

## <a name="BKMK_services"></a>Gerenciar serviços e configurar alertas de serviço  
Nesta seção, Aprenda como iniciar, parar, reiniciar, pausar ou retomar os serviços que são exibidos na **Services** lado a lado em páginas do grupo de funções e servidores no Gerenciador do servidor. Você também pode configurar os serviços que você deseja receber alertas nas miniaturas no painel do Gerenciador do servidor.  

> [!NOTE]  
> Você não pode alterar o tipo de início para os serviços, as dependências de serviço, as opções de recuperação ou outras propriedades de serviço no bloco serviços no Gerenciador do servidor. Para alterar outras propriedades de serviço além do status, abra o snap-in **Serviços**. Um atalho para abrir o **Services** snap-in está disponível na **ferramentas** menu no Gerenciador do servidor.  

#### <a name="to-start-stop-restart-pause-or-resume-a-service"></a>Para iniciar, parar, reiniciar, pausar ou continuar um serviço  

1.  No console do Gerenciador do servidor, abra qualquer página, exceto o dashboard (em outras palavras, qualquer função ou grupo home page).  

2.  No bloco **Serviços** da função ou do grupo, clique com o botão direito do mouse em um serviço.  

3.  No menu de contexto, clique na ação que você deseja executar no serviço. Se o serviço for interrompido, a única ação que você poderá executar é iniciar o serviço. De forma semelhante, se o serviço for pausado, a única ação que você poderá executar é continuar o serviço.  

4.  Ao iniciar, parar, reiniciar, pausar ou retomar um serviço, observe que o valor da coluna **Status** do serviço é alterado no bloco **Serviços** .  

#### <a name="to-configure-service-alerts-highlighted-in-thumbnails"></a>Para configurar os alertas de serviço destacados em miniaturas  

1.  Na página do dashboard, em uma miniatura do bloco **Grupos de Funções e Servidores**, clique na linha **Serviços**.  

2.  No **modo de exibição de detalhes de serviços** caixa de diálogo, selecione os tipos de inicialização para serviços sobre os quais você deseja ser alertado. Por padrão, **automática (início atrasado)** e **automático** estão selecionados.  

3.  Selecione os status de serviço sobre o qual você deseja ser alertado. Por padrão, **Todos** está selecionado.  

4.  Selecione serviços sobre os quais você deseja ser alertado. Por padrão, **Todos** está selecionado.  

5.  Selecione os servidores associados com a função ou grupo para o qual você deseja receber alertas sobre os serviços. Por padrão, **Todos** está selecionado.  

6.  À medida que você altera os critérios para os quais os alertas são exibidos, o número de alertas que é exibido no painel de resultados na parte interior da caixa de diálogo pode ser alterado. Clique em **Ocultar Alertas** para ocultar todos os alertas mais antigos que o período atual e evitar que eles afetem a contagem de alertas exibida na miniatura de origem.  

7.  Clique em **Mostrar Tudo** para retornar os alertas ocultos à lista.  

8.  Clique em **Okey** para salvar suas alterações, feche o **exibição de detalhe** caixa de diálogo e exibir alterações do alerta de serviço na miniatura de origem.  

## <a name="BKMK_copy"></a>Exibir e copiar entradas de eventos, serviços ou desempenho  
Você pode copiar as propriedades de entrada de eventos, serviços ou desempenho em ambos os **visualização detalhada** caixas de diálogo e o **eventos** e **desempenho** blocos para uma função ou grupo. Uma entrada de evento ou desempenho com o botão direito e, em seguida, clique em **cópia**.  

O bloco **Eventos** também permite visualizar as propriedades de eventos na metade inferior do bloco selecionando um evento na lista. Para copiar as propriedades mostradas na visualização, clique com botão direito no painel de visualização e, em seguida, clique em **cópia**.  

## <a name="see-also"></a>Consulte também  
[Server Manager](server-manager.md)  
[Filtrar, classificar e consultar dados em blocos do Gerenciador do servidor](filter-sort-and-query-data-in-server-manager-tiles.md)  
