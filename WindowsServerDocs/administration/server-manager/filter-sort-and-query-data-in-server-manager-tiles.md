---
title: Filtrar, classificar e consultar dados em blocos do Gerenciador do Servidor
description: Gerenciador do Servidor
ms.topic: article
ms.assetid: 8786f791-73e5-4c75-8d12-46e88a196976
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b41dd1dc2ad0da3a4eec6105105ab5ada5469dc7
ms.sourcegitcommit: 53d526bfeddb89d28af44210a23ba417f6ce0ecf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87895783"
---
# <a name="filter-sort-and-query-data-in-server-manager-tiles"></a>Filtrar, classificar e consultar dados em blocos do Gerenciador do Servidor

>Aplica-se a: Windows Server (Canal Semestral), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

No Windows Server, os blocos no Gerenciador do Servidor permitem filtrar e classificar dados e criar e salvar consultas personalizadas. Você pode classificar, usar filtros de palavra-chave e executar consultas em entradas de lista nos blocos eventos, desempenho, Analisador de Práticas Recomendadas, serviços e funções e recursos nas páginas função de servidor ou grupo no Gerenciador do Servidor.

Este tópico inclui as seções a seguir.

-   [Filtrar entradas de listas em blocos](#BKMK_tiles)

-   [classificar entradas de lista em blocos](#BKMK_sort)

-   [criar e executar consultas personalizadas em dados de bloco](#BKMK_query)

## <a name="filter-list-entries-in-tiles"></a><a name=BKMK_tiles></a>Filtrar entradas de listas em blocos
A caixa de texto **Filtrar** é um modo rápido de reduzir a lista de entradas que são exibidas em um bloco para apenas aquelas entradas que contém uma cadeia de caracteres de texto especificada.

#### <a name="to-apply-a-filter-to-the-list-of-entries-in-a-tile"></a>Para aplicar um filtro à lista de entradas em um bloco

1.  Abra uma página de grupo de função ou de servidor no Gerenciador do Servidor.

2.  Na caixa de texto de **filtro** de um bloco eventos, desempenho, analisador de práticas recomendadas, serviços ou funções e recursos, digite uma cadeia de caracteres na qual você deseja filtrar.

    por exemplo, se você quiser ver apenas os eventos com uma ID de evento 1014, digite **1014** na caixa de texto de **filtro** . Todos os eventos coletados que contêm a cadeia de caracteres de texto **1014** em pelo menos um campo retornam como resultados.

3.  Observe que o filtro altera o texto de descrição sob o título do bloco. Em vez de **Todos** os resultados, ele exibe **Resultados filtrados**.

4.  Para limpar o filtro, exclua a cadeia de caracteres de texto na caixa de filtro ou clique em **X**.

## <a name="sort-list-entries-in-tiles"></a><a name=BKMK_sort></a>classificar entradas de lista em blocos
Classifique as entradas de lista em blocos de Gerenciador do Servidor clicando em títulos de coluna. Ao clicar em um cabeçalho de coluna pela primeira vez, os valores da coluna são classificados em ordem alfanumérica crescente (seta para cima); ao clicar novamente os valores da coluna são classificados em ordem alfanumérica decrescente (seta para baixo).

## <a name="create-and-run-custom-queries-on-tile-data"></a><a name=BKMK_query></a>criar e executar consultas personalizadas em dados de bloco
Você pode criar consultas personalizadas nos blocos eventos, desempenho, Analisador de Práticas Recomendadas, serviços ou funções e recursos no Gerenciador do Servidor. Por padrão, a área da barra de ferramentas do bloco na qual você seleciona critérios para criar uma consulta personalizada é oculta; clique em **expandir** (botão de divisa na borda direita da barra de ferramentas do bloco) para exibir os critérios de consulta.

#### <a name="to-create-a-custom-query-for-tile-data"></a>Para criar uma consulta personalizada para os dados do bloco

1.  Abra uma página de grupo de função ou de servidor no Gerenciador do Servidor.

2.  Em um bloco eventos, desempenho, Analisador de Práticas Recomendadas, serviços ou funções e recursos, expanda a área criação de consultas clicando em **expandir**.

3.  Clique em **Adicionar critérios** para abrir uma lista de atributos (ou campos) que se aplicam às entradas no bloco.

4.  Selecione os critérios a serem adicionados. Quando tiver terminado, clique em **Adicionar**. Os critérios que você selecionou são adicionados à área de construção de consulta.

5.  Clique no operador de hipertexto para selecionar um operador. Para critérios numéricos ou de data e hora, por exemplo, o padrão é **menor que ou igual a**.

6.  Especifique valores aceitáveis para os critérios. Por exemplo, se você selecionou **data e hora**, forneça uma data no formato *m/d/AAAA*.

7.  Repita essas etapas da etapa 3 em diante para adicionar mais critérios à sua consulta.

    Você pode adicionar duplicatas dos critérios que já estão em sua consulta, mas as duplicatas são adicionadas à consulta com o operador **ou**.

    por exemplo, para consultar as IDs de evento 1003 ou 1014, você primeiro adicionaria os critérios de ID à sua consulta, tornaria o valor de ID igual a **1003**e, em seguida, adicionará um segundo critério de ID à sua consulta, tornando o valor da segunda ID igual a **1014**. A consulta resultante é **e ID é igual a 1003 ou a ID é igual a 1014**.

8.  Quando tiver terminado de adicionar critérios e especificar operadores e valores, clique em **Salvar** para salvar a consulta.

9. Digite um nome amigável para a consulta. Por exemplo, a consulta criada na etapa anterior pode ser nomeada **Eventos de licenciamento**.

10. Quando tiver terminado de visualizar os resultados da consulta, clique em **Limpar Tudo** para limpar todos os filtros e consultas, e exibir todas as entradas na lista.

11. Para executar uma consulta salva, clique em **Consultas de Pesquisa Salvas**, e clique no nome da consulta salva que deseja executar.

12. Para excluir uma consulta salva, clique em **Consultas de Pesquisa Salvas**, e clique no **X** junto ao nome da consulta salva que deseja excluir.

## <a name="see-also"></a>Consulte Também
[Gerenciador do servidor](server-manager.md) 
 [Exibir e configurar dados de desempenho, eventos e serviços](view-and-configure-performance-event-and-service-data.md)



