---
title: Gerenciar Clusters de Failover no Windows Admin Center
description: Gerenciar Clusters de Failover no Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 06/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 63f5fca8e7ef63200e01cfc6e00a979e7f045b51
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/12/2019
ms.locfileid: "9296668"
---
# Gerenciar Clusters de Failover no Windows Admin Center

>Aplicável a: Windows Admin Center, Windows Admin Center Preview

> [!Tip]
> Novo no Windows Admin Center?
> [Saiba mais sobre o Windows Admin Center](../understand/windows-admin-center.md) ou [Baixe agora](https://aka.ms/windowsadmincenter).

## Gerenciar clusters de failover
[Clustering de failover](https://docs.microsoft.com/windows-server/failover-clustering/failover-clustering-overview) é um recurso do Windows Server que permite que você agrupe vários servidores em um cluster tolerante a falhas para aumentar a disponibilidade e escalabilidade de aplicativos e serviços, como o servidor de arquivos de escalabilidade horizontal, Hyper-V e Microsoft SQL Server.

Embora você possa gerenciar nós de cluster de failover como servidores individuais, adicionando-as como [conexões de servidor](manage-servers.md) no Windows Admin Center, você também pode adicioná-los como clusters de Failover para exibir e gerenciar recursos de cluster, armazenamento, rede, nós, funções, virtuais computadores e switches virtuais.

![Tela de visão geral do cluster de failover](../media/manage-failover-clusters/fcm-overview.png)

## Adicionar um cluster de failover no Windows Admin Center
Para adicionar um cluster no Windows Admin Center:

1. Clique em **+ Adicionar** em todas as conexões.
2. Escolha esta opção Adicionar uma **Conexão de Failover**.
3. Digite o nome do cluster e, se solicitado, as credenciais para usar.
4. Você terá a opção de adicionar os nós do cluster como conexões de servidor individual no Centro de administração do Windows.
5. Clique em **Enviar** para concluir.

O cluster será adicionado à sua lista de conexão na página de visão geral. Clique nele para se conectar ao cluster.

> [!NOTE]
> Você também pode gerenciar hiperconvergente clusterizados adicionando o cluster como uma [conexão de Cluster hiperconvergente](manage-hyper-converged.md) no Centro de administração do Windows.

## Ferramentas

As seguintes ferramentas estão disponíveis para conexões de cluster de failover:

| Ferramenta | Descrição |
| ---- | ----------- |
| Visão geral | Exibir detalhes de cluster de failover e gerenciar recursos de cluster |
| Discos | Cluster de modo de exibição compartilhado discos e volumes |
| Redes | Exibir redes no cluster |
| Nós | Exibir e gerenciar nós de cluster |
| Funções | Gerenciar funções de cluster ou criar uma função vazia |
| Atualizações | Gerenciar atualizações de Cluster-Aware (requer [CredSSP](../understand/faq.md#does-windows-admin-center-use-credssp)) |
| [Máquinas virtuais](manage-virtual-machines.md) | Exibir e gerenciar máquinas virtuais |
| Switches virtuais | Exibir e gerenciar switches virtuais |

## Aguarde mais

Gerenciamento de cluster de failover no Windows Admin Center está ativamente em desenvolvimento e novos recursos serão adicionados no futuro próximo. Você pode exibir o status e vote recursos em UserVoice:

|Solicitação de recurso|
|-------|
| [Mostrar mais informações de disco em cluster](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31740424--cluster-more-disk-info-in-failover-cluster-manag) |
| [Ações de cluster adicionais de suporte](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/33558076--fcm-full-csv-management-cycle-in-one-place) |
| [Suporte a clusters convergidos executando o Hyper-V e servidor de arquivos de escalabilidade horizontal em clusters diferentes](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31729741--cluster-support-for-converged-architecture) |
| [Cache de bloco CSV de modo de exibição](https://windowsserver.uservoice.com/forums/295071-management-tools/suggestions/31669477--cluster-csv-block-cache) |
| [Ver todas as ou propor novo recurso](https://windowsserver.uservoice.com/forums/295071/filters/top?category_id=319162&query=%5Bcluster%5D) |