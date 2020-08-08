---
title: Atualizando clusters de failover usando o mesmo hardware
description: Este artigo descreve como atualizar um cluster de failover de 2 nós usando o mesmo hardware
manager: eldenc
ms.topic: article
author: johnmarlin-msft
ms.author: johnmar
ms.date: 02/28/2019
ms.localizationpriority: medium
ms.openlocfilehash: 3e25660eda2d21658f01fe1d8a01ae86a4b42116
ms.sourcegitcommit: 68444968565667f86ee0586ed4c43da4ab24aaed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87990951"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>Atualizando clusters de failover no mesmo hardware

> Aplica-se a: Windows Server 2019, Windows Server 2016

Um cluster de failover é um conjunto de computadores independentes que trabalham em conjunto para aumentar a disponibilidade de aplicativos e serviços. Os servidores clusterizados (chamados de nós) são conectados por cabos físicos e por software. Se um dos nós do cluster falhar, o outro nó começará a fornecer o serviço (um processo conhecido como failover). Os usuários vivenciam um mínimo de interrupções no serviço.

Este guia descreve as etapas para atualizar os nós de cluster para o Windows Server 2019 ou o Windows Server 2016 de uma versão anterior usando o mesmo hardware.

## <a name="overview"></a>Visão geral

A atualização do sistema operacional em um cluster de failover existente só tem suporte ao passar do Windows Server 2016 para o Windows 2019.  Se o cluster de failover estiver executando uma versão anterior, como o Windows Server 2012 R2 e anterior, a atualização enquanto os serviços de cluster estão em execução não permitirá a junção de nós.  Se estiver usando o mesmo hardware, as etapas podem ser seguidas para obtê-lo para a versão mais recente.

Antes de qualquer atualização do cluster de failover do, consulte o [conteúdo de atualização do Windows Server](../upgrade/upgrade-overview.md).  Ao atualizar um Windows Server in-loco, você passa de uma versão do sistema operacional existente para uma versão mais recente enquanto permanece no mesmo hardware. O Windows Server pode ser atualizado no local pelo menos um e, às vezes, duas versões para frente. Por exemplo, o Windows Server 2012 R2 e o Windows Server 2016 podem ser atualizados in-loco para o Windows Server 2019.  Lembre-se também de que o [Assistente de migração de cluster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) pode ser usado, mas tem suporte apenas para até duas versões de volta. O gráfico a seguir mostra os caminhos de atualização para o Windows Server. As setas apontando para baixo representam o caminho de atualização com suporte que se move de versões anteriores até o Windows Server 2019.

![Diagrama de atualização in-loco](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

As etapas a seguir são um exemplo de saída de um servidor de cluster de failover do Windows Server 2012 para o Windows Server 2019 usando o mesmo hardware.

Antes de iniciar qualquer atualização, verifique se um backup atual, incluindo o estado do sistema, foi feito.  Verifique também se todos os drivers e firmware foram atualizados para os níveis certificados para o sistema operacional que você usará.  Essas duas observações não serão abordadas aqui.

No exemplo a seguir, o nome do cluster de failover é CLUSTER e os nomes de nó são NODE1 e NODE2.

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>Etapa 1: remover o primeiro nó e atualizar para o Windows Server 2016

1. Em Gerenciador de Cluster de Failover, dissipe todos os recursos de NODE1 para NODE2 clicando com o botão direito do mouse no nó e selecionando **Pausar** e **drenar funções**.  Como alternativa, você pode usar o comando [Suspend-CLUSTERNODE](/powershell/module/failoverclusters/suspend-clusternode)do PowerShell.

    ![Nó de dreno](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. Remova NODE1 do cluster clicando com o botão direito do mouse no nó e selecionando **mais ações** e **remover**.  Como alternativa, você pode usar o comando [Remove-CLUSTERNODE](/powershell/module/failoverclusters/remove-clusternode)do PowerShell.

    ![Nó de dreno](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. Como precaução, desanexe o NODE1 do armazenamento que você está usando.  Em alguns casos, desconectar os cabos de armazenamento do computador será suficiente.  Verifique com seu fornecedor de armazenamento as etapas de desanexação adequadas, se necessário.  Dependendo do seu armazenamento, isso pode não ser necessário.

4. Reconstrua o NODE1 com o Windows Server 2016.  Certifique-se de ter adicionado todas as funções, os recursos, os drivers e as atualizações de segurança necessários.

5. Crie um novo cluster chamado CLUSTER1 com NODE1.  Abra Gerenciador de Cluster de Failover e, no painel **Gerenciamento** , escolha **criar cluster** e siga as instruções no assistente.

    ![Nó de dreno](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. Depois que o cluster for criado, as funções precisarão ser migradas do cluster original para esse novo cluster.  No novo cluster, clique com o botão direito do mouse no nome do cluster (CLUSTER1) e selecione **mais ações** e **copie as funções do cluster**.  Acompanhe o assistente para migrar as funções.

    ![Nó de dreno](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  Depois que todos os recursos tiverem sido migrados, desligue o NODE2 (cluster original) e desconecte o armazenamento para não causar nenhuma interferência.  Conecte o armazenamento ao NODE1.  Quando todos estiverem conectados, coloque todos os recursos online e verifique se eles estão funcionando como deveria.

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>Etapa 2: recompilar o segundo nó ao Windows Server 2019

Depois de verificar que tudo está funcionando como deveria, o NODE2 pode ser recriado para o Windows Server 2019 e ingressado no cluster.

1. Execute uma instalação limpa do Windows Server 2019 no NODE2. Certifique-se de ter adicionado todas as funções, os recursos, os drivers e as atualizações de segurança necessários.

2. Agora que o cluster original (CLUSTER) não existe, você pode deixar o novo nome de cluster como CLUSTER1 ou voltar para o nome original.  Se você quiser voltar para o nome original, siga estas etapas:

   a. Em NODE1, em Gerenciador de Cluster de Failover com o botão direito do mouse, clique no nome do cluster (CLUSTER1) e escolha **Propriedades**.

   b. Na guia **geral** , renomeie o CLUSTER para cluster.

   c. Ao escolher OK ou aplicar, você verá o Popup da caixa de diálogo abaixo.

    ![Nó de dreno](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. O serviço de cluster será interrompido e precisará ser iniciado novamente para que a renomeação seja concluída.

3. No NODE1, abra Gerenciador de Cluster de Failover.  Clique com o botão direito do mouse em **nós** e selecione **adicionar nó**.  Passe o assistente adicionando NODE2 ao cluster.

4. Anexe o armazenamento ao NODE2. Isso pode incluir a reconexão dos cabos de armazenamento.

5. Dissipe todos os recursos de NODE1 para NODE2 clicando com o botão direito do mouse no nó e selecionando **Pausar** e **drenar funções**.  Como alternativa, você pode usar o comando [Suspend-CLUSTERNODE](/powershell/module/failoverclusters/suspend-clusternode)do PowerShell.  Verifique se todos os recursos estão online e se estão funcionando como deveria.

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>Etapa 3: recriar o primeiro nó para o Windows Server 2019

1. Remova NODE1 do cluster e desconecte o armazenamento do nó da maneira que você fez anteriormente.

2. Recompile ou atualize o NODE1 para o Windows Server 2019.  Certifique-se de ter adicionado todas as funções, os recursos, os drivers e as atualizações de segurança necessários.

3. Anexe novamente o armazenamento e adicione NODE1 de volta ao cluster.

4. Mova todos os recursos para NODE1 e verifique se eles são online e funcionam conforme necessário.

5. O nível funcional atual do cluster permanece no Windows 2016.  Atualize o nível funcional para o Windows 2019 com o comando do PowerShell [Update-CLUSTERFUNCTIONALLEVEL](/powershell/module/failoverclusters/update-clusterfunctionallevel).

Agora você está executando com um cluster de failover do Windows Server 2019 totalmente funcional.

## <a name="additional-notes"></a>Observações adicionais

- Conforme explicado anteriormente, desconectar o armazenamento pode ou não ser necessário.  Em nossa documentação, desejamos não ter cuidado.  Entre em contato com seu fornecedor de armazenamento.
- Se o ponto de partida for um cluster do Windows Server 2008 ou 2008 R2, poderá ser necessária uma execução adicional por meio de etapas.
- Se o cluster estiver executando máquinas virtuais, certifique-se de atualizar o nível da máquina virtual quando o nível funcional do cluster tiver sido feito com o comando [Update-VMVERSION](/powershell/module/hyper-v/update-vmversion)do PowerShell.
- Observe que, se você estiver executando um aplicativo como SQL Server, Exchange Server, etc, o aplicativo não será migrado com o assistente para copiar funções de cluster.  Você deve consultar o fornecedor do aplicativo para obter as etapas de migração adequadas do aplicativo.