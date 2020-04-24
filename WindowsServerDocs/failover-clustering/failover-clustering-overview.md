---
ms.assetid: c9844427-27cf-4d76-b5bb-e06368b092f7
title: Clustering de failover
ms.prod: windows-server
ms.topic: landing-page
manager: lizross
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/06/2019
ms.localizationpriority: high
ms.openlocfilehash: b646890ebc8b8e64d84e6d448ce4acb393422009
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/23/2020
ms.locfileid: "80827709"
---
# <a name="failover-clustering-in-windows-server"></a>Clustering de failover no Windows Server

> Aplica-se a: Windows Server 2019, Windows Server 2016

Um cluster de failover é um conjunto de computadores independentes que trabalham em conjunto para aumentar a disponibilidade e escalabilidade de funções de cluster (antigamente chamadas de aplicações e serviços de cluster). Os servidores clusterizados (chamados de nós) são conectados por cabos físicos e por software. Se um ou mais dos nós do cluster falhar, o outro nó começará a fornecer o serviço (um processo conhecido como failover). Além disso, as funções de cluster são monitoradas de maneira proativa para verificar se estão funcionando adequadamente. Se o funcionamento não estiver correto, elas serão reiniciadas ou movidas para outro nó.

Os clusters de failover também fornecem a funcionalidade CSV (Volume Compartilhado Clusterizado) que, por sua vez, oferece um namespace consistente distribuído, o qual pode ser usado para acessar o armazenamento compartilhado em todos os nós. Com o recurso Clustering de Failover, os usuários passam pelo mínimo de interrupções no serviço.

O Clustering de Failover tem muitas aplicações práticas, dentre elas:

* Armazenamento de compartilhamento de arquivos altamente disponível ou continuamente disponível para aplicativos como máquinas virtuais do Microsoft SQL Server e do Hyper-V
* Funções clusterizadas com alta disponibilidade que são executadas em servidores físicos ou em máquinas virtuais instaladas em servidores que executam o Hyper-V

| **Noções básicas**                                                               |  **Planejamento**                          |  **Implantação**       |
| -------------                                                                |  --------------                        | --------------------- |
| [Novidades no clustering de failover](whats-new-in-failover-clustering.md)    | [Planejamento dos requisitos de hardware do clustering de failover e opções de armazenamento](clustering-requirements.md)  | [Criação de um cluster de failover](create-failover-cluster.md) |
| [Servidor de arquivos de escalabilidade horizontal para dados de aplicativos](sofs-overview.md)               | [Usar CSVs (Volumes Compartilhados do Cluster)](failover-cluster-csvs.md) | [Implantar um servidor de arquivos com dois nós](../storage/storage-spaces/storage-spaces-direct-in-vm.md) |
|  [Quorum de cluster e pool](../storage/storage-spaces/understand-quorum.md)   |  [Usar clusters de máquina virtual de convidado com espaços de armazenamento diretos](../storage/storage-spaces/storage-spaces-direct-in-vm.md)       | [Pré-configurar os objetos de computador do cluster no Active Directory Domain Services](prestage-cluster-adds.md) |
| [Reconhecimento de domínio de falha](fault-domains.md)                                 |                                 | [Configuração de contas de cluster no Active Directory](configure-ad-accounts.md) |
| [SMB Multichannel simplificado e redes de cluster de várias NICs](smb-multichannel.md) |                       | [Gerenciar quorum e testemunhas](manage-cluster-quorum.md) |
| [Balanceamento de carga de VM](vm-load-balancing-overview.md)                         |                             | [Implantar uma testemunha de nuvem](deploy-cloud-witness.md) |
| [Conjuntos de cluster](../storage/storage-spaces/cluster-sets.md)                  |                             |[Implantar testemunha de compartilhamento de arquivos](file-share-witness.md) |
| [Afinidade de cluster](cluster-affinity.md)                                     |                            | [Atualizações sem interrupção do sistema operacional do cluster](cluster-operating-system-rolling-upgrade.md) |
|                                                                             |                            | [Atualizar um cluster de failover no mesmo hardware](upgrade-option-same-hardware.md) |
|                                                                            |                             | [Implantar um cluster desanexado do Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn265970\(v%3dws.11\))

|**Gerenciar**  |  **Ferramentas e configurações**  |  **Recursos da comunidade**       |
| ------------- |  -------------- | --------------------- |
| [Atualização com suporte a cluster](cluster-aware-updating.md)    |   [Cmdlets do PowerShell para clustering de failover](https://docs.microsoft.com/powershell/module/failoverclusters/?view=win10-ps)      |  [Fórum sobre alta disponibilidade (clustering)](https://go.microsoft.com/fwlink/p/?LinkId=230641)       |
|  [Serviço de integridade](health-service-overview.md)   |   [Cmdlets do PowerShell para atualização com suporte a cluster](https://docs.microsoft.com/powershell/module/clusterawareupdating/?view=win10-ps)      | [Blog da equipe de clustering de failover e balanceamento de carga de rede](https://blogs.msdn.com/b/clustering/)        |
|  [Migração de domínio do cluster](cluster-domain-migration.md)   |         |         |
|  [Solução de problemas usando o Relatório de Erros do Windows](troubleshooting-using-wer-reports.md)   |         |         |
