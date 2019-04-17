---
title: Atualizando Clusters de Failover usando o mesmo Hardware
ms.prod: windows-server-threshold
ms.manager: eldenc
ms.technology: failover-clustering
ms.topic: article
author: johnmarlin-msft
ms.date: 02/28/2019
description: Este artigo descreve a atualização de um Cluster de Failover de 2 nós usando o mesmo hardware
ms.localizationpriority: medium
ms.openlocfilehash: 0bfeb05c8cbc205745dc16bc7ef04052481668ea
ms.sourcegitcommit: 2c2027b597e2483eea8967d0710d65c2247b6751
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/01/2019
ms.locfileid: "9121261"
---
# Atualizando Clusters de Failover no mesmo hardware

> Aplicável a: Windows Server 2019, Windows Server 2016

Um cluster de failover é um grupo de computadores independentes que trabalham juntos para aumentar a disponibilidade de aplicativos e serviços. Os servidores clusterizados (chamados de nós) são conectados por cabos físicos e por software. Se um de nós do cluster falhar, outro nó começará a fornecer o serviço (um processo conhecido como failover). Os usuários passam pelo mínimo de interrupções no serviço.

Este guia descreve as etapas para atualizar os nós de cluster para o Windows Server 2019 ou Windows Server 2016 de uma versão anterior usando o mesmo hardware.

## Visão geral

Atualizar o sistema operacional em um failover existente cluster só é compatível ao passar do Windows Server 2016 para Windows 2019.  Se o cluster de failover estiver executando uma versão anterior, tais como o Windows Server 2012 R2 e anteriores, enquanto os serviços de cluster estiver executando a atualização não permitirá ingressar em nós juntos.  Se usando o mesmo hardware, etapas podem ser executadas obtê-lo para a versão mais recente.  

Antes de qualquer atualização de seu cluster de failover, consulte o [Centro de atualização do Windows](https://www.microsoft.com/upgradecenter).  Ao atualizar um Windows Server no lugar, você migrará de uma versão de sistema operacional existente para uma versão mais recente, mantendo o mesmo hardware. Windows Server pode ser atualizado in-loco pelo menos uma e, às vezes, duas versões para frente. Por exemplo, Windows Server 2012 R2 e Windows Server 2016 podem ser atualizados para Windows Server 2019.  Também tenha em mente que o [Assistente de migração do Cluster](https://blogs.msdn.microsoft.com/clustering/2012/06/25/how-to-move-highly-available-clustered-vms-to-windows-server-2012-with-the-cluster-migration-wizard/) pode ser usado, mas só é suportada até duas versões novamente. O gráfico a seguir mostra os caminhos de atualização para o Windows Server. Setas apontando para baixo representam o caminho de atualização com suporte, mover de versões anteriores até Windows Server 2019.

![Diagrama de atualização in-loco](media\In-Place-Upgrade\In-Place-Upgrade-1.png)

As etapas a seguir são um exemplo de passar de um servidor de cluster de failover do Windows Server 2012 para o Windows Server 2019 usando o mesmo hardware.  

Antes de começar qualquer atualização, certifique-se de um backup atual, incluindo o estado do sistema, foi feito.  Verifique também se todos os drivers e firmware foram atualizados para os níveis de certificados para o sistema operacional que você usará.  Essas duas anotações não serão abordadas aqui.

No exemplo a seguir, o nome do cluster de failover é o CLUSTER e os nomes de nó são NODE1 e NODE2.

## Etapa 1: Remover primeiro nó e atualizar para o Windows Server 2016

1. No Gerenciador de Cluster de Failover, esvazie todos os recursos do Nó1 para o Nó2 por direito do mouse clicando no nó e selecionando **Pausar** e **Esvaziar funções**.  Como alternativa, você pode usar o comando do PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).

    ![Esvaziar o nó](media\In-Place-Upgrade\In-Place-Upgrade-2.png)

2. Remova NODE1 do Cluster por clicando no nó e selecionando **Mais ações** e **Evict**direito do mouse.  Como alternativa, você pode usar o comando do PowerShell [REMOVE-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/remove-clusternode).

    ![Esvaziar o nó](media\In-Place-Upgrade\In-Place-Upgrade-3.png)

3. Como precaução, desanexe NODE1 do armazenamento que você está usando.  Em alguns casos, desconectar os cabos de armazenamento do computador será suficiente.  Verifique com seu fornecedor de armazenamento para as etapas adequadas desconexão se necessário.  Dependendo de seu armazenamento, isso pode não ser necessário.

4. Recompile NODE1 com o Windows Server 2016.  Certifique-se de que você adicionou todas as funções necessárias, recursos, drivers e atualizações de segurança.

5. Crie um novo cluster chamado CLUSTER1 com NODE1.  Abra o Gerenciador de Cluster de Failover e no painel de **gerenciamento** , escolha **Criar Cluster** e siga as instruções no assistente.

    ![Esvaziar o nó](media\In-Place-Upgrade\In-Place-Upgrade-4.png)

6. Depois que o Cluster é criado, as funções precisarão ser migrados do cluster original para esse novo cluster.  No novo cluster, direito do mouse clique no nome do cluster (CLUSTER1) e selecionando **Mais ações** e **Funções de Cluster de cópia**.  Acompanhar no Assistente para migrar as funções.

    ![Esvaziar o nó](media\In-Place-Upgrade\In-Place-Upgrade-5.png)

7.  Depois que todos os recursos foram migrados, desligue NODE2 (cluster original) e desconecte o armazenamento para não causar qualquer interferência.  Conecte o armazenamento ao NODE1.  Depois que tudo estiver conectado, colocar os recursos online e verifique se que eles estão funcionando como deve.

## Etapa 2: Recompilar o segundo nó para Windows Server 2019

Depois que você verificou que tudo está funcionando como deveria, NODE2 podem ser recompilado para Windows Server 2019 e ingressou no cluster.

1. Execute uma instalação limpa do Windows Server 2019 no NODE2. Certifique-se de que você adicionou todas as funções necessárias, recursos, drivers e atualizações de segurança.

2. Agora que desapareceu no cluster original (CLUSTER), você pode deixar o nome do novo cluster como CLUSTER1 ou voltar para o nome original.  Se você quiser voltar para o nome original, siga estas etapas:
   
   a. No NODE1, clique no nome do cluster (CLUSTER1) no Gerenciador de Cluster de Failover direito do mouse e escolha **Propriedades**.
   
   b. Na guia **Geral** , renomear o cluster ao CLUSTER.

   c. Ao escolher Okey ou aplicar, você verá o abaixo pop-up de caixa de diálogo.

    ![Esvaziar o nó](media\In-Place-Upgrade\In-Place-Upgrade-6.png)

    d. O serviço de Cluster será interrompido e precisava ser iniciado novamente para renomear concluir.

3. No NODE1, abra o Gerenciador de Cluster de Failover.  Clique direito do mouse em **nós** e selecione **Adicionar nó**.  Percorra o Assistente de adição de NODE2 ao Cluster.

4. Anexe o armazenamento à NODE2. Isso pode incluir reconectar os cabos de armazenamento. 

5. Esvazie todos os recursos do Nó1 para o Nó2 por direito do mouse clicando no nó e selecionando **Pausar** e **Esvaziar funções**.  Como alternativa, você pode usar o comando do PowerShell [SUSPEND-CLUSTERNODE](https://docs.microsoft.com/powershell/module/failoverclusters/suspend-clusternode).  Verifique se todos os recursos estão online e eles estão funcionando como deve.

## Etapa 3: Recompilar o primeiro nó ao Windows Server 2019

1. Remover NODE1 do cluster e desconecte o armazenamento do nó da maneira na qual você anteriormente.

2. Recompile ou atualizar NODE1 para o Windows Server 2019.  Certifique-se de que você adicionou todas as funções necessárias, recursos, drivers e atualizações de segurança.

3. Anexe novamente o armazenamento e adicione NODE1 volta ao cluster.

4. Mova todos os recursos para NODE1 e garantir que eles vêm on-line e funcionam conforme necessário.

5. O nível funcional do cluster atual permanece em Windows 2016.  Atualize o nível funcional de 2019 do Windows com o comando do PowerShell [UPDATE-CLUSTERFUNCTIONALLEVEL](https://docs.microsoft.com/powershell/module/failoverclusters/update-clusterfunctionallevel).

Agora você está executando com um Cluster de Failover do Windows Server 2019 totalmente funcionais.

## Observações adicionais

- Conforme explicado anteriormente, desconectar o armazenamento pode ou não ser necessário.  Em nossa documentação, queremos que seja cauteloso.  Consulte seu fornecedor de armazenamento.
- Se o ponto de partida é o Windows Server 2008 ou 2008 R2 clusters, uma execução adicional por meio das etapas pode ser necessárias.
- Se estiver executando o cluster de máquinas virtuais, certifique-se de que atualizar o nível de máquina virtual depois que o nível funcional do cluster foi feito com o comando do PowerShell [VMVERSION de atualização](https://docs.microsoft.com/powershell/module/hyper-v/update-vmversion).
- Observe que se você estiver executando um aplicativo, como o SQL Server, Exchange Server, etc, o aplicativo não será migrado com o Assistente para funções de Cluster de cópia.  Você deve consultar o fornecedor do aplicativo para obter as etapas de migração adequada do aplicativo.