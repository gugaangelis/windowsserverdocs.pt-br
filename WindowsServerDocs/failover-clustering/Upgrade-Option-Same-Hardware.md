---
title: Atualizando Clusters de Failover usando o mesmo Hardware
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: Este artigo descreve como atualizar um Cluster de Failover de 2 nós usando o mesmo hardware
ms.localizationpriority: medium
ms.openlocfilehash: 6787d852cc5075e306373a163814135190f27fd6
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/20/2019
ms.locfileid: "67280240"
---
# <a name="upgrading-failover-clusters-on-the-same-hardware"></a>Atualizando Clusters de Failover no mesmo hardware

> Aplica-se a: Windows Server 2019, Windows Server 2016

Um cluster de failover é um conjunto de computadores independentes que trabalham em conjunto para aumentar a disponibilidade de aplicativos e serviços. Os servidores clusterizados (chamados de nós) são conectados por cabos físicos e por software. Se um dos nós do cluster falhar, o outro nó começará a fornecer o serviço (um processo conhecido como failover). Os usuários vivenciam um mínimo de interrupções no serviço.

Este guia descreve as etapas para atualizar os nós de cluster para o Windows Server 2019 ou Windows Server 2016 de uma versão anterior usando o mesmo hardware.

## <a name="overview"></a>Visão geral

Atualização do sistema operacional em um failover existente cluster tem suporte apenas ao passar de Windows Server 2016 para Windows de 2019.  Se o cluster de failover está em execução em uma versão anterior, como o Windows Server 2012 R2 e versões anteriores, atualizando enquanto os serviços de cluster estão em execução não permitirá que unir nós.  Se usar o mesmo hardware, etapas podem ser executadas para obtê-lo para a versão mais recente.  

Antes de qualquer atualização do seu cluster de failover, consulte a [Centro de atualização do Windows](https://www.microsoft.com/upgradecenter).  Quando você atualiza um Windows Server no local, você move de uma versão de sistema operacional existente para uma versão mais recente, mantendo o mesmo hardware. Windows Server pode ser atualizado in-loco pelo menos um e, às vezes, duas versões para frente. Por exemplo, Windows Server 2012 R2 e Windows Server 2016 podem ser atualizados no local para o Windows Server 2019.  Além disso, tenha em mente que o [Assistente de migração de Cluster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) pode ser usado, mas só há suporte para até duas versões de volta. O gráfico a seguir mostra os caminhos de atualização para o Windows Server. Setas apontando para baixo representam o caminho de atualização com suporte, movimentação de versões anteriores até 2019 do Windows Server.

![Diagrama de atualização in-loco](media/In-Place-Upgrade/In-Place-Upgrade-1.png)

As etapas a seguir são um exemplo de andamento de um servidor de cluster de failover do Windows Server 2012 para Windows Server 2019 usando o mesmo hardware.  

Antes de iniciar qualquer atualização, verifique se foi feito um backup atual, incluindo o estado do sistema.  Também verifique se todos os drivers e firmware foram atualizados para os níveis de certificados para o sistema operacional que você usará.  Estas duas notas não serão abordadas aqui.

No exemplo a seguir, o nome do cluster de failover é o CLUSTER e os nomes de nó são NODE1 e NODE2.

## <a name="step-1-evict-first-node-and-upgrade-to-windows-server-2016"></a>Etapa 1: Remova o primeiro nó e atualizar para o Windows Server 2016

1. No Gerenciador de Cluster de Failover, esvaziar todos os recursos do Nó1 para o NODE2 pelo botão direito do mouse clicando no nó e selecionando **pausa** e **esvaziar funções**.  Como alternativa, você pode usar o comando do PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).

    ![Drenagem de nó](media/In-Place-Upgrade/In-Place-Upgrade-2.png)

2. Remova o NODE1 do Cluster pelo botão direito do mouse no nó de clicando e selecionando **mais ações** e **Evict**.  Como alternativa, você pode usar o comando do PowerShell [REMOVE-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode).

    ![Drenagem de nó](media/In-Place-Upgrade/In-Place-Upgrade-3.png)

3. Como precaução, desanexe NODE1 do armazenamento que você está usando.  Em alguns casos, desconectar os cabos de armazenamento da máquina será suficiente.  Verifique com seu fornecedor de armazenamento para obter as etapas de desanexação adequado se necessário.  Dependendo do seu armazenamento, isso pode não ser necessário.

4. Recompile NODE1 com o Windows Server 2016.  Verifique se que você tiver adicionado todas as funções necessárias, recursos, drivers e atualizações de segurança.

5. Crie um novo cluster chamado CLUSTER1 com NODE1.  Abra o Gerenciador de Cluster de Failover e, na **Management** painel, escolha **criar Cluster** e siga as instruções no assistente.

    ![Drenagem de nó](media/In-Place-Upgrade/In-Place-Upgrade-4.png)

6. Depois que o Cluster é criado, as funções precisam ser migrados do cluster original para esse novo cluster.  No novo cluster, com o botão direito do mouse clique no nome do cluster (CLUSTER1) e selecionando **mais ações** e **copiar funções de Cluster**.  Acompanhe no Assistente para migrar as funções.

    ![Drenagem de nó](media/In-Place-Upgrade/In-Place-Upgrade-5.png)

7.  Depois que todos os recursos foram migrados, desligue NODE2 (cluster original) e desconectar o armazenamento de modo a não causar qualquer interferência.  Conecte o armazenamento NODE1.  Depois que tudo está conectado, colocar os recursos online e garantir que eles estejam funcionando como deve.

## <a name="step-2-rebuild-second-node-to-windows-server-2019"></a>Etapa 2: Recompilar o segundo nó para o Windows Server 2019

Uma vez que você verificou que tudo está funcionando como deveria, NODE2 pode ser recompilado para Windows Server 2019 e associado ao Cluster.

1. Execute uma instalação limpa do Windows Server 2019 no NODE2. Verifique se que você tiver adicionado todas as funções necessárias, recursos, drivers e atualizações de segurança.

2. Agora que o cluster original (CLUSTER) é concluído, você pode deixar o novo nome de cluster CLUSTER1 ou volte para o nome original.  Se você quiser voltar para o nome original, siga estas etapas:
   
   a. No NODE1, no Gerenciador de Cluster de Failover direito do mouse clique no nome do cluster (CLUSTER1) e escolha **propriedades**.
   
   b. Sobre o **geral** guia, renomeie o cluster para CLUSTER.

   c. Ao escolher Okey ou aplicar, você verá o abaixo de pop-up de caixa de diálogo.

    ![Drenagem de nó](media/In-Place-Upgrade/In-Place-Upgrade-6.png)

    d. O serviço de Cluster será interrompido e precisava ser iniciado novamente para concluir a renomeação.

3. No NODE1, abra o Gerenciador de Cluster de Failover.  Clique do botão direito do mouse em **nós** e selecione **adicionar nó**.  Usar o Assistente para adicionar o NODE2 ao Cluster.

4. Anexe o armazenamento para o NODE2. Isso pode incluir reconectando os cabos de armazenamento. 

5. Esvaziar todos os recursos do Nó1 para o NODE2 pelo botão direito do mouse clicando no nó e selecionando **pausa** e **esvaziar funções**.  Como alternativa, você pode usar o comando do PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).  Verifique se todos os recursos estão online e eles estão funcionando como deve.

## <a name="step-3-rebuild-first-node-to-windows-server-2019"></a>Etapa 3: Recompilar o primeiro nó para o Windows Server 2019

1. Remover NODE1 do cluster e desconecte o armazenamento do nó da maneira do que você anteriormente.

2. Recompilar ou atualizar NODE1 para o Windows Server 2019.  Verifique se que você tiver adicionado todas as funções necessárias, recursos, drivers e atualizações de segurança.

3. Anexar novamente o armazenamento e adicione NODE1 volta para o cluster.

4. Mova todos os recursos para NODE1 e certifique-se de que ficam online e de função conforme necessário.

5. O nível funcional do cluster atual permanece no Windows 2016.  Atualizar o nível funcional para 2019 do Windows com o comando do PowerShell [UPDATE-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel).

Agora você está executando com um Cluster de Failover do Windows Server 2019 totalmente funcional.

## <a name="additional-notes"></a>Observações adicionais

- Conforme explicado anteriormente, desconectar o armazenamento pode ou não ser necessário.  Em nossa documentação, queremos err com cautela.  Consulte seu fornecedor de armazenamento.
- Se o seu ponto de partida é o Windows Server 2008 ou 2008 R2 clusters, uma execução adicional por meio das etapas pode ser necessários.
- Se o cluster estiver executando máquinas virtuais, certifique-se de atualizar o nível da máquina virtual depois que o nível funcional do cluster foi feito com o comando do PowerShell [VMVERSION atualização](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion).
- Observe que se você estiver executando um aplicativo como o SQL Server, Exchange Server, etc, o aplicativo não será migrado com o Assistente para copiar funções de Cluster.  Você deve consultar o fornecedor do aplicativo para obter as etapas de migração adequada do aplicativo.