---
title: Gerenciar clusters de failover com o centro de administração do Windows
description: Gerenciar clusters de failover com o centro de administração do Windows (projeto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: b7f015ac4c9906447069501bf0922b36306a51d7
ms.sourcegitcommit: 083ff9bed4867604dfe1cb42914550da05093d25
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75950491"
---
# <a name="manage-failover-clusters-with-windows-admin-center"></a>Gerenciar clusters de failover com o centro de administração do Windows

>Aplica-se a: Windows Admin Center, Visualização do Windows Admin Center

> [!Tip]
> Novo no Windows Admin Center?
> [Baixe ou saiba mais sobre o centro de administração do Windows](../overview.md).

## <a name="managing-failover-clusters"></a>Gerenciando clusters de failover
O [clustering de failover](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) é um recurso do Windows Server que permite agrupar vários servidores juntos em um cluster tolerante a falhas para aumentar a disponibilidade e a escalabilidade de aplicativos e serviços, como o servidor de arquivos de escalabilidade horizontal, o Hyper-V e o Microsoft SQL Server.

Embora você possa gerenciar nós de cluster de failover como servidores individuais adicionando-os como [conexões de servidor](manage-servers.md) no centro de administração do Windows, você também pode adicioná-los como clusters de failover para exibir e gerenciar recursos de cluster, armazenamento, rede, nós, funções, máquinas virtuais e comutadores virtuais.

![Tela de visão geral do cluster de failover](../media/manage-failover-clusters/fcm-overview.png)

## <a name="adding-a-failover-cluster-to-windows-admin-center"></a>Adicionando um cluster de failover ao centro de administração do Windows
Para adicionar um cluster ao centro de administração do Windows:

1. Clique em **+ Adicionar** em todas as conexões.
2. Escolha Adicionar uma **conexão de failover**.
3. Digite o nome do cluster e, se solicitado, as credenciais a serem usadas.
4. Você terá a opção de adicionar os nós de cluster como conexões de servidor individuais no centro de administração do Windows.
5. Clique em **Enviar** para concluir.

O cluster será adicionado à sua lista de conexões na página Visão geral. Clique para se conectar ao cluster.

> [!NOTE]
> Você também pode gerenciar o cluster hiperconvergente adicionando o cluster como uma [conexão de cluster hiperconvergente](manage-hyper-converged.md) no centro de administração do Windows.

## <a name="tools"></a>Ferramentas

As seguintes ferramentas estão disponíveis para conexões de cluster de failover:

| Ferramenta | Descrição |
| ---- | ----------- |
| Visão geral | Exibir detalhes do cluster de failover e gerenciar recursos de cluster |
| Discos | Exibir volumes e discos compartilhados do cluster |
| Redes | Exibir redes no cluster |
| Nós | Exibir e gerenciar nós de cluster |
| Funções | Gerenciar funções de cluster ou criar uma função vazia |
| Atualizações | Gerenciar atualizações com suporte a cluster (requer [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)) |
| [Máquinas Virtuais](manage-virtual-machines.md) | Exibir e gerenciar máquinas virtuais |
| Switches virtuais | Exibir e gerenciar comutadores virtuais |

## <a name="more-coming"></a>Mais em breve

O gerenciamento de cluster de failover no centro de administração do Windows está ativamente em desenvolvimento e novos recursos serão adicionados em breve. Você pode exibir o status e votar para recursos no UserVoice:

|Solicitação de recurso|
|-------|
| [Mostrar mais informações de disco clusterizado](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Suporte a ações de cluster adicionais](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Suporte a clusters convergidos que executam o Hyper-V e Servidor de Arquivos de Escalabilidade Horizontal em clusters diferentes](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Exibir cache de bloco CSV](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Ver todo ou propor novo recurso](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |