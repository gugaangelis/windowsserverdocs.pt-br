---
title: Gerenciar Clusters de Failover com o Windows Admin Center
description: Gerenciar Clusters de Failover com o Windows Admin Center (projeto Paulo)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: f7e14581f7f6b14b0cf39308de236b68a07e8c9f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59824067"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>Gerenciar Clusters de Failover com o Windows Admin Center

>Aplica-se a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novo no Windows Admin Center?
> [Saiba mais sobre o Windows Admin Center](../understand/windows-admin-center.md) ou [Baixe agora](https://aka.ms/windowsadmincenter).

## <a name="managing-failover-clusters"></a>Gerenciar clusters de failover
[Clustering de failover](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) é um recurso do Windows Server que permite a você agrupar vários servidores em um cluster e tolerante a falhas para aumentar a disponibilidade e escalabilidade de aplicativos e serviços, como o servidor de arquivos de escalabilidade horizontal, Hyper-V e Microsoft SQL Server.

Embora você possa gerenciar nós de cluster de failover como servidores individuais, adicionando-os como [conexões de servidor](manage-servers.md) no do Windows Admin Center, você também pode adicioná-los como clusters de Failover para exibir e gerenciar recursos de cluster, armazenamento, rede, nós, funções, as máquinas virtuais e comutadores virtuais.

![Tela de visão geral do cluster de failover](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>A adição de um cluster de failover do Windows Admin Center
Para adicionar um cluster para Windows Admin Center:

1. Clique em **+ adicionar** em todas as conexões.
2. Optar por adicionar um **Conexão de Failover**.
3. Digite o nome do cluster e, se solicitado, as credenciais a usar.
4. Você terá a opção de adicionar os nós do cluster como conexões de servidor individuais em Windows Admin Center.
5. Clique em **enviar** para concluir.

O cluster será adicionado à sua lista de conexão na página de visão geral. Clique nele para se conectar ao cluster.

> [!NOTE]
> Você também pode gerenciar hiperconvergente em cluster adicionando o cluster como um [conexão de Cluster Hyper-Converged](manage-hyper-converged.md) no Windows Admin Center.

## <a name="tools"></a>Ferramentas

As seguintes ferramentas estão disponíveis para conexões de cluster de failover:

| Ferramenta | Descrição |
| ---- | ----------- |
| Visão geral | Exibir detalhes do cluster de failover e gerenciar recursos de cluster |
| Discos | Compartilhado do cluster de modo de exibição discos e volumes |
| Redes | Exibir redes no cluster |
| Nós | Exibir e gerenciar nós de cluster |
| Funções | Gerenciar funções de cluster ou criar uma função vazia |
| Atualizações | Gerenciar atualizações com suporte a Cluster |
| [Máquinas Virtuais](manage-virtual-machines.md) | Exibir e gerenciar máquinas virtuais |
| Comutadores Virtuais | Exibir e gerenciar os comutadores virtuais |

## <a name="more-coming"></a>Aguarde mais

Gerenciamento de cluster de failover no Windows Admin Center ativamente está em desenvolvimento e novos recursos serão adicionados no futuro próximo. Você pode exibir o status e votar em recursos no UserVoice:

|Solicitação de recurso|
|-------|
| [Mostrar mais informações de disco clusterizado](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Suporte a ações de cluster adicionais](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Suporte a clusters convergentes executando o Hyper-V e o servidor de arquivos de escalabilidade horizontal em diferentes clusters](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Cache de bloco CSV do modo de exibição](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Ver todos os ou propor o novo recurso](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |