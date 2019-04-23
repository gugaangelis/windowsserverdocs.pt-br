---
title: Exibir detalhes e notificações de tarefas
description: Gerenciador do Servidor
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-server-manager
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 95117407-2dd3-4f9a-841f-4331be3544c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 44fd23b917d08aad663c0d3fb8da4bebef47783e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59831507"
---
# <a name="view-task-details-and-notifications"></a>Exibir detalhes e notificações de tarefas

>Aplica-se a: Windows Server 2016

No Gerenciador do servidor no Windows Server 2012 R2 ou Windows Server 2012, quando você executa tarefas de gerenciamento como a adição de funções e recursos, iniciar serviços, a atualização de dados que são exibidos no console do Gerenciador do servidor ou criar um grupo personalizado de servidores, um notificação é exibida na **notificações** área do cabeçalho de console do Gerenciador do servidor. As notificações e o **detalhes de tarefa** caixa de diálogo que você pode abrir na **notificações** menu clicando no ícone de sinalizador exibem o status de solicitações ou tarefas do usuário, mostram se uma tarefa falhou e ajudá-lo Solucione problemas apontando para mensagens de erro detalhadas sobre falhas de tarefas.

## <a name="the-notifications-area"></a>A área de Notificações
O **notificações** área na barra de menus do Gerenciador do servidor, marcada por um ícone de sinalizador, exibe os resultados das tarefas que você inicia no Gerenciador do servidor. As notificações informam se as tarefas que você iniciou no Gerenciador do servidor foram bem-sucedidas ou não. Quando há notificações disponíveis para você exibir, o número de notificações disponíveis é exibido ao lado do ícone de sinalizador. Se uma tarefa falhou, pôde ser concluída apenas parcialmente (por exemplo, se não foi possível concluí-la em todos os servidores remotos nos quais você desejava executar a tarefa) ou foi concluída com avisos, o sinalizador **Notificações** fica vermelho. As tarefas para as quais são exibidas notificações incluem as seguintes.

-   Atualização manual dos dados exibidos no Gerenciador do servidor (serão exibidas notificações para atualizações automáticas somente se elas falharem)

-   iniciando ou parando serviços

-   Instalar ou desinstalar funções, serviços de função e recursos

-   Iniciar uma verificação do analisador de práticas recomendadas (BPA)

-   adição de servidores remotos para gerenciar (são exibidas notificações para falhas ao contatar ou atualizar os dados exibidos para servidores remotos)

Os itens no menu **Notificações** mostram uma barra de progresso, uma breve descrição da tarefa, o nome do servidor de destino da tarefa (ou um dos servidores de destino, se foram selecionados vários servidores de destino), um link para um controle ou uma caixa de diálogo relacionada, se aplicável, e um menu **Tarefas** . O menu **Tarefas** mostra os comandos que se aplicam à notificação ativa (a notificação sobre a qual o cursor do mouse está focalizado). Por exemplo, se você parar um serviço, poderá clicar em **Reiniciar** no menu **Tarefas** na notificação para reiniciar o serviço.

As notificações são particularmente úteis para instalar ou desinstalar funções, serviços de função e recursos. Por exemplo, se você iniciar a instalação de um recurso em um servidor remoto, você pode fechar o Assistente de recursos e adicionar funções durante a instalação ainda está em andamento, mas a tarefa ativa permanecerá na **notificações** lista. O **notificações** item mostra uma barra de progresso para a instalação e permite reabrir a adicionar funções e recursos do assistente, se necessário, clicando em **adicionar funções e recursos do assistente**. Os itens nessa lista informam se uma instalação falhou ou se são necessárias etapas de configuração adicionais para concluir a implantação do recurso.

As notificações também desempenham um papel importante na solução de problemas com tarefas ou processos no Gerenciador do servidor. Para obter mais informações sobre como usar mensagens na **notificações** área e **detalhes de tarefa** caixa de diálogo para solucionar problemas de processos ou tarefas sem êxito, consulte o [Gerenciador do servidor Guia de solução](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx).

Para excluir uma notificação de que você não deseja mais ver a partir de **notificações** listar, focalize o cursor do mouse sobre a notificação e, em seguida, clique em **remover tarefa** (**X**).

## <a name="viewing-and-troubleshooting-tasks-by-using-task-details"></a>Exibindo e Solucionando problemas de tarefas usando detalhes da tarefa
O **detalhes de tarefa** comando na parte inferior da **notificações** menu abre a **detalhes da tarefa** caixa de diálogo que fornece descrições completas de eventos de tarefa (a partir de, parada, avisos, êxitos ou falhas). Como outros controles de lista no Gerenciador do servidor, como **eventos**, **Services**, e **Best Practices Analyzer** blocos, você pode filtrar e criar consultas para serem executadas nas tarefas que são exibidos na **detalhes de tarefa** caixa de diálogo. (para obter mais informações sobre como filtrar e criar consultas em controles de lista, consulte [filtrar, classificar e consulta dados em blocos do Gerenciador do servidor](filter-sort-and-query-data-in-server-manager-tiles.md).) No painel superior, você pode revisar as notificações como elas foram exibidas no menu **Notificações** e ver quantas notificações foram geradas sobre a mesma tarefa. seleção de uma notificação no painel superior exibe os detalhes completos sobre a notificação no painel inferior.

O painel inferior é especialmente útil para solucionar problemas de tarefas com falha. Se o Gerenciador do servidor não pode se conectar a ou obter dados para um servidor que seja membro do pool de servidores, as entradas nesse painel frequentemente contêm mensagens detalhadas, incluindo o texto completo do Windows remote Management (WinRM) de base, rede ou problemas de segurança que impedir que o Gerenciador do servidor se comunicar com um servidor de destino.

## <a name="see-also"></a>Consulte também
[Filtrar, classificar e consultar dados em blocos do Gerenciador do servidor](filter-sort-and-query-data-in-server-manager-tiles.md)
[guia de solução de problemas do Gerenciador do servidor](https://social.technet.microsoft.com/wiki/contents/articles/13443.windows-server-2012-server-manager-troubleshooting-guide-part-i-overview.aspx)
