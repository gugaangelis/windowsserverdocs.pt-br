---
title: Exibir e configurar dados de serviço e eventos de desempenho
description: Gerenciador do Servidor
ms.topic: article
ms.assetid: ccd59c35-4dbf-48e7-88a4-c519c00184d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e7fbf4c96213d7db042143c1da8065f87e642f47
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87993163"
---
# <a name="view-and-configure-performance-event-and-service-data"></a>View and Configure Performance, Event, and Service Data

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Este tópico descreve como exibir e configurar as entradas do log de eventos, os contadores de desempenho e os alertas de serviço que são exibidos para servidores locais e remotos no Gerenciador do Servidor.

Os dados de log de evento, serviço e desempenho são exibidos em dois locais no console do Gerenciador do Servidor no Windows Server.

-   No painel, você pode clicar nas linhas **eventos**, **desempenho**e **Serviços** de miniaturas para configurar o evento, o desempenho e os dados de log do serviço que deseja ver para as funções, todo o pool de Servidores Gerenciador do servidor, grupos de servidor criados pelo usuário e o servidor local. Clicar nas linhas de hipertexto abre as caixas de diálogo **exibição de detalhes** que permitem especificar os dados sobre os quais você deseja ser alertado no painel. Depois de configurar o evento, o serviço e os dados de log de desempenho que você deseja realçar nas miniaturas do painel, as entradas de log que correspondem aos critérios especificados são listadas na parte inferior das caixas de diálogo **exibição de detalhes** .

-   Os blocos **Eventos**, **Serviços** e **Desempenho** fazem parte das home pages de funções e grupos. Os comandos no menu **Tarefas** desses blocos permitem especificar os dados que serão coletados dos servidores gerenciados. Os blocos incluem filtros e consultas para limitar ainda mais as entradas de log exibidas no bloco, se desejado.

Este tópico inclui as seções a seguir.

-   [O que são miniaturas?](#BKMK_thumb)

-   [Exibir e configurar eventos](#BKMK_events)

-   [Exibir e configurar contadores de desempenho](#BKMK_perf)

-   [Gerenciar serviços e configurar alertas de serviços](#BKMK_services)

-   [Exibir e copiar entradas de eventos ou desempenho](#BKMK_copy)

## <a name="what-are-thumbnails"></a><a name=BKMK_thumb></a>O que são miniaturas?
As *miniaturas* são exibidas no painel de Gerenciador do servidor para cada função (a miniatura de uma função reflete os dados coletados sobre todos os servidores no pool de Gerenciador do servidor que estão executando a função), para cada grupo de servidores, para o grupo **todos os** servidor (todos os servidores no pool de Gerenciador do servidor) e para o servidor local. Depois que Gerenciador do Servidor obtém dados de servidores gerenciados, as miniaturas são criadas automaticamente para funções que estão sendo executadas em servidores no pool de servidores.

Se o console do Gerenciador do Servidor estiver em execução em um computador cliente como parte do Ferramentas de Administração de Servidor Remoto, não haverá nenhuma miniatura do **servidor local** .

A miniatura faz uma exibição rápida do status e da capacidade de gerenciamento de funções, servidores e grupos de servidores. A linha de título em miniatura altera a cor (e os números realçados são exibidos na margem esquerda) quando eventos, contadores de desempenho, resultados de Analisador de Práticas Recomendadas, serviços ou problemas de capacidade de gerenciamento geral atendem aos critérios que você configura nas caixas de diálogo de **exibição de detalhes** abertas clicando em linhas de miniatura. A tabela a seguir descreve os dados exibidos nas miniaturas.

|Linha de miniaturas|Descrição|
|---------|--------|
|Capacidade de gerenciamento|A capacidade de gerenciamento de um servidor inclui várias medidas: se o servidor está online ou offline, se ele está acessível e relatando dados para Gerenciador do Servidor, se o usuário que está conectado ao computador local tem direitos de usuário adequados para acessar ou gerenciar o servidor remoto, se o servidor remoto está executando todo o software necessário para gerenciá-lo remotamente ou se o servidor está configurado de forma a permitir que ele seja consultado e gerenciado usando Gerenciador do Servidor. Os únicos dados de capacidade de gerenciamento que Gerenciador do Servidor podem coletar de um servidor que executa o Windows Server 2003 é se o servidor está online ou offline. Para obter informações detalhadas sobre os erros de status de gerenciamento e como resolvê-los, consulte o [Fórum de Gerenciador do servidor](/answers/topics/windows-server-manager.html).|
|Eventos|Você pode configurar a linha de **Eventos** de uma miniatura para exibir alertas quando os eventos registrados correspondem aos níveis de severidade, às fontes, aos períodos de tempo, servidores ou IDs de eventos que você especificar. Exiba detalhes sobre os eventos e altere os alertas que você deseja ver clicando na linha **eventos** e abra a caixa de diálogo **exibição de detalhes de eventos** para a função ou grupo de servidores.|
|Serviços|Você pode configurar a linha de **Serviços** para exibir alertas quando forem encontrados serviços em uma função ou grupo de servidores que correspondam a tipos de inicialização, status de serviço, nomes de serviço e servidores que você especificar na caixa de diálogo **exibição de detalhes de serviços** .<p>Depois que um servidor tiver sido adicionado ao pool de servidores do Gerenciador do Servidor, os alertas de serviço sobre o serviço de detecção de hardware do shell poderão ser exibidos se não houver usuários conectados ao servidor gerenciado. Isso ocorre porque o serviço de detecção do hardware do shell é executado apenas quando usuários estão conectados ao servidor gerenciado ou a uma sessão de área de trabalho remota no servidor gerenciado. Para impedir a exibição de alertas do serviço de detecção do hardware do shell nesse caso, clique em **Serviços** nas miniaturas dos grupos de servidores, incluindo o grupo **Todos os servidores**. Na caixa de diálogo **exibição de detalhes dos serviços** , na lista suspensa **Serviços** , desmarque a caixa de seleção para **detecção de hardware do Shell**e clique em **OK**.|
|Desempenho|Você pode configurar a linha de **desempenho** para exibir alertas para uma função ou grupo de servidores quando ocorrerem alertas de desempenho que correspondam a tipos de recursos, servidores ou períodos de tempo que você especificar na caixa de diálogo **exibição de detalhes de desempenho** .<p>Por padrão, os contadores de desempenho estão desativados. Os servidores gerenciados que estão executando sistemas operacionais mais recentes do que o Windows Server 2003 e para os quais os contadores de desempenho não foram iniciados, normalmente mostram erros de status de gerenciamento de **contadores de desempenho online não iniciados** no bloco **servidores** de páginas de função ou grupo. Para ativar os contadores de desempenho para servidores gerenciados, na página **todos os servidores** , clique com o botão direito do mouse em entradas no bloco **desempenho** que mostram um valor de **status de contador** de **desativado**e clique em **Iniciar contadores de desempenho**. Você também pode iniciar contadores de desempenho clicando com o botão direito do mouse em entradas para servidores no bloco **servidores** de páginas de função ou grupo e, em seguida, clicando em **Iniciar contadores de desempenho**.|
|Resultados BPA|Você pode configurar a linha de **resultados do BPA** para exibir alertas para uma função ou grupo de servidores quando forem encontrados resultados da verificação do BPA que correspondam aos níveis de severidade, servidores ou categorias do BPA que você especificar na caixa de diálogo **exibição de detalhes dos resultados do BPA** .|

## <a name="view-and-configure-events"></a><a name=BKMK_events></a>Exibir e configurar eventos
Nesta seção, saiba como configurar quais dados de log de eventos são coletados dos servidores no pool de servidores do Gerenciador do Servidor e quais eventos você deseja realçar em miniaturas.

> [!NOTE]
> Os eventos sobre os quais você é alertado em miniaturas são um subconjunto do total de eventos que você instrui Gerenciador do Servidor a coletar de servidores gerenciados. Embora a alteração de critérios de evento na caixa de diálogo **configurar dados de eventos** em blocos de **eventos** possa alterar os números de alertas que você vê no painel Gerenciador do servidor, alterar os critérios de alerta de eventos em miniaturas não tem nenhum efeito sobre os dados do log de eventos que são coletados dos servidores gerenciados.

#### <a name="to-configure-the-events-collected-from-managed-servers"></a>Para configurar os eventos coletados dos servidores gerenciados

1.  No console do Gerenciador do Servidor, abra qualquer página, exceto o painel. É possível configurar os eventos que deseja coletar dos servidores gerenciados no bloco **Eventos** nas páginas da função, do grupo de servidores ou do servidor local.

2.  No menu **Tarefas** do bloco **Eventos**, clique em **Configurar Dados do Evento**.

3.  Selecione os níveis de severidade de evento que você deseja que sejam coletados dos servidores no grupo selecionado. Por padrão, os níveis de severidade **Crítico**, **Erro** e **Aviso** estão selecionados.

4.  Especifique um período de tempo durante o qual ocorrem os eventos. A duração padrão de eventos é 24 horas.

5.  Selecione os arquivos de log de eventos dos quais você deseja que os eventos sejam coletados. Os padrões são **Aplicativo**, **Configuração** e **Sistema**.

6.  Para salvar as alterações, clique em **OK** para fechar a caixa de diálogo **Configurar Dados do Evento**. Os dados do evento são atualizados automaticamente quando as alterações são salvas.

#### <a name="to-configure-the-events-highlighted-in-thumbnails"></a>Para configurar os eventos destacados em miniaturas

1.  se Gerenciador do Servidor já estiver aberto, vá para a próxima etapa. Se o Gerenciador do Servidor ainda não estiver aberto, abra-o de uma das maneiras a seguir.

    -   Na área de trabalho do Windows, inicie o Gerenciador do Servidor clicando em **Gerenciador do Servidor** na barra de tarefas do Windows.

    -   Na tela **Iniciar** do Windows, clique no bloco Gerenciador do servidor.

2.  Na página do dashboard, em uma miniatura do bloco **Grupos de Funções e Servidores**, clique na linha **Eventos**.

3.  Na caixa de diálogo **exibição de detalhes de eventos** , adicione um nível de severidade aos eventos que você deseja exibir. Por padrão, as miniaturas só exibem o realce de alerta para eventos críticos. Observe que o número de eventos exibidos na caixa de diálogo **exibição de detalhes** aumenta quando você adiciona um nível de severidade sobre o qual você deseja ser alertado.

4.  No campo **Fontes de evento**, selecione as fontes de evento para as quais deseja receber alerta. O padrão é **All**.

5.  Se essa miniatura for para uma função que está instalada em vários servidores ou em um grupo de vários servidores, você poderá selecionar os servidores para os quais deseja alertas de eventos na lista suspensa **servidores** .

6.  No campo **período de tempo** , especifique um período de tempo até 1440 minutos, 24 horas ou 1 dia.

7.  No campo **IDs de Eventos**, digite os números das IDs dos eventos específicos para os quais deseja receber alerta. Você pode digitar um intervalo de IDs de eventos separados por um traço ( **-** ) e excluir IDs de eventos do intervalo digitando o traço antes da ID do evento ou do intervalo de IDs de evento que você deseja excluir. Por exemplo, o valor **1,3,5-99,-76** significa que os alertas são acionados para as IDs de eventos 1 e 3 e para todos os eventos com IDs entre 5 e 99, exceto para a ID de evento 76.

8.  À medida que você altera os critérios para os quais os alertas são exibidos, o número de alertas de eventos que é exibido no painel de resultados na parte interior da caixa de diálogo pode ser alterado. Selecione entradas na lista e clique em **ocultar alertas** para impedir que eles afetem a contagem de alertas exibida na miniatura de origem. Pressione e segure **Ctrl** enquanto seleciona os alertas para selecionar vários ao mesmo tempo. Você pode fazer isso para os alertas que correspondem aos seus critérios de alerta de eventos, mas que você não precisa ver.

9. Clique em **Mostrar Tudo** para retornar os alertas ocultos à lista.

10. Clique em **OK** para salvar as alterações, feche a caixa de diálogo **exibição de detalhes** e exiba as alterações de alerta de evento na miniatura de origem.

## <a name="view-and-configure-performance-log-data"></a><a name=BKMK_perf></a>Exibir e configurar os dados do log de desempenho
Nesta seção, saiba como configurar quais dados de log de desempenho são coletados dos servidores no pool de Servidores Gerenciador do Servidor e quais alertas do contador de desempenho você deseja realçados em miniaturas.

Por padrão, os contadores de desempenho estão desativados. Os servidores gerenciados que estão executando sistemas operacionais mais recentes do que o Windows Server 2003 e para os quais os contadores de desempenho não foram iniciados, normalmente mostram erros de status de gerenciamento de **contadores de desempenho online não iniciados** no bloco **servidores** de páginas de função ou grupo. Para ativar os contadores de desempenho para servidores gerenciados, na página **todos os servidores** , clique com o botão direito do mouse em entradas no bloco **desempenho** que mostram um valor de **status de contador** de **desativado**e clique em **Iniciar contadores de desempenho**. Você também pode iniciar contadores de desempenho clicando com o botão direito do mouse em entradas para servidores no bloco **servidores** de páginas de função ou grupo e, em seguida, clicando em **Iniciar contadores de desempenho**.

> [!NOTE]
> Os alertas de desempenho que você exibe em miniaturas são um subconjunto dos dados do contador de desempenho total que você instrui Gerenciador do Servidor a coletar de servidores gerenciados. Embora a alteração dos critérios de alerta de desempenho na caixa de diálogo **configurar alertas de desempenho** em blocos de **desempenho** possa alterar os números de alertas que você vê no painel de Gerenciador do servidor, alterar os critérios de alerta de desempenho em miniaturas não tem efeito sobre os dados de log de desempenho coletados dos servidores gerenciados.
>
> Por isso, a duração máxima dos dados de desempenho que você pode exibir nas miniaturas não pode ser maior que o período de exibição de gráfico máximo configurado na caixa de diálogo **Configurar alertas de desempenho**. Por exemplo, se o valor do **período de exibição do grafo** em **configurar alertas de desempenho** for **1 dia**, o valor máximo do campo **período de tempo** em uma caixa de diálogo exibição de detalhes de **desempenho** que você abriu do painel Gerenciador do Servidor poderá ser de **1 dia**, **24 horas**ou **1.440 minutos**.

#### <a name="to-configure-the-performance-log-data-collected-from-managed-servers"></a>Para configurar os dados do log de desempenho coletados dos servidores gerenciados

1.  No console do Gerenciador do Servidor, abra qualquer página, exceto o painel. É possível configurar os dados de desempenho que deseja coletar dos servidores gerenciados no bloco **Desempenho** nas páginas da função, do grupo de servidores ou do servidor local.

2.  Para coletar os dados do log de desempenho dos servidores gerenciados, os contadores de desempenho devem estar ligados. Se os contadores de desempenho estiverem desativados, clique com o botão direito do mouse em uma entrada na lista bloco de **desempenho** e clique em **Iniciar contadores de desempenho**. A coleta de dados do contador de desempenho pode levar algum tempo, dependendo do número de servidores dos quais os dados são coletados e da largura de banda de rede disponível. Exiba o status na coluna **Status do Contador**.

3.  No menu **Tarefas** do bloco **Desempenho**, clique em **Configurar Alertas de Desempenho**.

4.  para os servidores no grupo selecionado ou que estão executando a função selecionada, especifique em qual porcentagem de uso da CPU você deseja que os alertas do contador de desempenho sejam coletados pelo Gerenciador do Servidor. O padrão é 85%.

5.  Especifique a memória restante disponível, em megabytes, que os servidores devem ter antes que os alertas do contador de desempenho possam ser coletados. O valor padrão é 2 MB.

6.  Especifique o período de tempo a ser exibido nos gráficos para os recursos **Uso de CPU** e **Memória Disponível** no bloco **Desempenho** na página selecionada. O período padrão é de um dia. Clique em **Salvar**.

    Observe que o número de alertas de desempenho no bloco **Desempenho** e o mapeamento dos alertas ao longo do tempo conforme exibido pelo gráfico podem mudar depois que você clica em **Salvar**.

    > [!NOTE]
    > para máquinas virtuais que têm o [memória dinâmica](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ff817651(v=ws.10)) ativado, aumentar o limite de alertas de desempenho pode resultar em alertas falsos positivos.

7.  Para atualizar a lista de alertas de desempenho coletados dos servidores, no menu **Tarefas**, clique em **Atualizar**.

#### <a name="to-configure-the-performance-alerts-highlighted-in-thumbnails"></a>Para configurar os alertas de desempenho destacados em miniaturas

1.  Na página do dashboard, em uma miniatura do bloco **Grupos de Funções e Servidores**, clique na linha **Desempenho**.

2.  Na caixa de diálogo **exibição de detalhes de desempenho** , marque ou desmarque as caixas de seleção dos limites de desempenho do recurso sobre os quais você deseja ser alertado no campo tipo de **recurso** . Observe que o número de alertas de desempenho exibido na caixa de diálogo **exibição de detalhes** pode aumentar quando você adiciona um limite de desempenho de recurso sobre o qual você deseja ser alertado.

3.  Se essa miniatura for para uma função que está instalada em vários servidores ou em um grupo de vários servidores, você poderá selecionar os servidores para os quais deseja alertas de desempenho na lista suspensa **servidores** .

4.  No campo **período de tempo** , especifique um período de tempo até 1440 minutos, 24 horas ou 1 dia.

5.  À medida que você altera os critérios para os quais os alertas são exibidos, o número de alertas que é exibido no painel de resultados na parte interior da caixa de diálogo pode ser alterado. Clique em **Ocultar Alertas** para ocultar todos os alertas mais antigos que o período atual e evitar que eles afetem a contagem de alertas exibida na miniatura de origem.

6.  Clique em **Mostrar Tudo** para retornar os alertas ocultos à lista.

7.  Clique em **OK** para salvar as alterações, feche a caixa de diálogo **exibição de detalhes** e exiba as alterações de alerta de desempenho na miniatura de origem.

#### <a name="to-view-the-properties-of-performance-alerts"></a>Para exibir as propriedades de alertas de desempenho

1.  Execute uma delas.

    -   Na página do dashboard, em uma miniatura do bloco **Grupos de Funções e Servidores**, clique na linha **Desempenho**.

    -   Abra a home page da função ou do grupo e localize o bloco **Desempenho** referente à função ou ao grupo.

2.  Clique duas vezes em um alerta de desempenho na lista para exibir suas propriedades. Se preferir, clique com o botão direito do mouse em um alerta de desempenho e clique em **Exibir Propriedades**.

3.  Na caixa de diálogo **Propriedades de Alertas de Desempenho**, selecione as entradas de log para exibir as informações sobre os processos associados à entrada na área **Processos**.

4.  Quando você tiver terminado de exibir as propriedades do alerta de desempenho, feche a caixa de diálogo.

### <a name="analyze-performance-data-and-solve-problems"></a>Analisar dados de desempenho e resolver problemas
para obter mais informações sobre como analisar os dados do contador de desempenho exibidos no Gerenciador do Servidor e solucionar problemas de desempenho em servidores gerenciados, consulte os recursos a seguir.

-   [Analisando dados de desempenho](https://go.microsoft.com/fwlink/?LinkId=239829)

-   [Resolvendo problemas de desempenho](https://go.microsoft.com/fwlink/?LinkId=239831)

para obter mais informações sobre ferramentas avançadas de monitoramento e análise de desempenho disponíveis para o Windows Server 2012 e versões posteriores do Windows Server, incluindo o supervisor de desempenho do servidor 3,0, consulte [desempenho](/previous-versions/windows/hardware/design/dn614608(v=vs.85)) no msdn.

## <a name="manage-services-and-configure-service-alerts"></a><a name=BKMK_services></a>Gerenciar serviços e configurar alertas de serviços
Nesta seção, saiba como iniciar, parar, reiniciar, pausar ou retomar os serviços que são exibidos no bloco **Serviços** nas páginas função e grupo de servidores no Gerenciador do servidor. Você também pode configurar os serviços sobre os quais você é alertado em miniaturas no painel Gerenciador do Servidor.

> [!NOTE]
> Você não pode alterar o tipo de início para serviços, dependências de serviço, opções de recuperação ou outras propriedades de serviço no bloco serviços no Gerenciador do Servidor. Para alterar outras propriedades de serviço além do status, abra o snap-in **Serviços**. Um atalho para abrir o snap-in **Serviços** está disponível no menu **ferramentas** no Gerenciador do servidor.

#### <a name="to-start-stop-restart-pause-or-resume-a-service"></a>Para iniciar, parar, reiniciar, pausar ou continuar um serviço

1.  No console do Gerenciador do Servidor, abra qualquer página, exceto o painel (em outras palavras, qualquer função ou grupo home page).

2.  No bloco **Serviços** da função ou do grupo, clique com o botão direito do mouse em um serviço.

3.  No menu de contexto, clique na ação que você deseja executar no serviço. Se o serviço for interrompido, a única ação que você poderá executar é iniciar o serviço. De forma semelhante, se o serviço for pausado, a única ação que você poderá executar é continuar o serviço.

4.  Ao iniciar, parar, reiniciar, pausar ou retomar um serviço, observe que o valor da coluna **Status** do serviço é alterado no bloco **Serviços**.

#### <a name="to-configure-service-alerts-highlighted-in-thumbnails"></a>Para configurar os alertas de serviço destacados em miniaturas

1.  Na página do dashboard, em uma miniatura do bloco **Grupos de Funções e Servidores**, clique na linha **Serviços**.

2.  Na caixa de diálogo **exibição de detalhes dos serviços** , selecione os tipos de inicialização dos serviços sobre os quais você deseja ser alertado. Por padrão, **automático (início atrasado)** e **automático** são selecionados.

3.  Selecione os status do serviço sobre o qual você deseja ser alertado. Por padrão, **Todos** está selecionado.

4.  Selecione os serviços sobre os quais você deseja ser alertado. Por padrão, **Todos** está selecionado.

5.  Selecione os servidores associados à função ou ao grupo para o qual você deseja receber alertas sobre serviços. Por padrão, **Todos** está selecionado.

6.  À medida que você altera os critérios para os quais os alertas são exibidos, o número de alertas que é exibido no painel de resultados na parte interior da caixa de diálogo pode ser alterado. Clique em **Ocultar Alertas** para ocultar todos os alertas mais antigos que o período atual e evitar que eles afetem a contagem de alertas exibida na miniatura de origem.

7.  Clique em **Mostrar Tudo** para retornar os alertas ocultos à lista.

8.  Clique em **OK** para salvar as alterações, feche a caixa de diálogo **exibição de detalhes** e exiba as alterações de alerta de serviço na miniatura de origem.

## <a name="view-and-copy-event-service-or-performance-entries"></a><a name=BKMK_copy></a>Exibir e copiar entradas de eventos, serviços ou desempenho
Você pode copiar propriedades de evento, serviço ou entrada de desempenho nas caixas de diálogo **exibição de detalhes** e nos blocos **eventos** e **desempenho** de uma função ou grupo. Clique com o botão direito do mouse em um evento ou entrada de desempenho e clique em **copiar**.

O bloco **Eventos** também permite visualizar as propriedades de eventos na metade inferior do bloco selecionando um evento na lista. Para copiar as propriedades mostradas na visualização, clique com o botão direito do mouse no painel de visualização e clique em **copiar**.

## <a name="see-also"></a>Consulte Também
[Gerenciador do servidor](server-manager.md) 
 [Filtrar, classificar e consultar dados em blocos de Gerenciador do servidor](filter-sort-and-query-data-in-server-manager-tiles.md)