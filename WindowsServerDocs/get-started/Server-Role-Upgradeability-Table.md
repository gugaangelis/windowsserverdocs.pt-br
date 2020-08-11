---
title: Matriz de atualização e migração da função de servidor para Windows Server 2016
description: Mostra quais funções de servidor podem ser atualizadas ou migradas para o Windows Server 2016.
ms.date: 10/05/2016
ms.topic: article
ms.assetid: 7e031a64-b1e6-4cf6-994a-e7c575835f6a
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: ee263e7de5d16c9992102b8c09cc709f51fa719d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87959294"
---
# <a name="server-role-upgrade-and-migration-matrix-for-windows-server-2016"></a>Matriz de atualização e migração da função de servidor para Windows Server 2016

>Aplica-se a: Windows Server 2016

A grade desta página explica suas opções de migração e atualização de funções de servidor, especificamente ao mover para o Windows Server 2016. Para obter guias de migração sobre funções específicas, visite [Migrar funções e recursos no Windows Server](./migrate-roles-and-features.md). Para saber mais sobre instalação e atualização, confira [Instalação, atualização e migração do Windows Server](./installation-and-upgrade.md).

|Função de servidor|Pode ser atualizado do Windows Server 2012 R2?|Pode ser atualizado do Windows Server 2012?|Migração compatível?|A migração pode ser concluída sem tempo de inatividade?|
|-------------------|----------|--------------|--------------|----------|
|Serviços de Certificados do Active Directory|    Sim|    Sim|    Sim|    Não|
|Active Directory Domain Services|    Sim|    Sim|    Sim|    Sim|
|Serviços de Federação do Active Directory|    Não|    Não|    Sim|    Não (os novos nós precisam ser adicionados ao farm)|
|Active Directory Lightweight Directory Services|    Sim|    Sim|    Sim|    Sim|
|Active Directory Rights Management Services|    Sim|    Sim|    Sim|    Não|
|Servidor DHCP|    Sim|    Sim|    Sim|    Sim|
|Servidor DNS|    Sim|    Sim|    Sim|    Não|
|Cluster de failover|Sim com o processo [Atualização sem interrupção do sistema operacional do cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md)contém o nó Pause-Drain, Evict, atualização para o Windows Server 2016 e o reingresso no cluster original. Sim, quando o servidor é removido pelo cluster para atualização e adicionado a um cluster diferente.|Não enquanto o servidor fizer parte de um cluster. Sim, quando o servidor é removido pelo cluster para atualização e adicionado a um cluster diferente.    |Sim|Não para Clusters de Failover no Windows Server 2012. Sim para Clusters de Failover do Windows Server 2012 R2 com máquinas virtuais do Hyper-V ou Clusters de Failover do Windows Server 2012 R2 executando a função Servidor de Arquivos de Escalabilidade Horizontal. confira [Atualização sem interrupção do sistema operacional do cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md).|
|Serviços de Arquivo e Armazenamento|    Sim|    Sim|    Varia de acordo com o sub-recurso|    Não|
|Hyper-V| Sim. (Quando o host faz parte de um cluster com o processo Atualização sem interrupção de sistema operacional do cluster, que inclui o nó Pause-Drain, Evict, atualização para o Windows Server 2016 e reingresso no cluster original.)|  Não|   Sim|  Não para Clusters de Failover no Windows Server 2012. Sim para Clusters de Failover do Windows Server 2012 R2 com máquinas virtuais do Hyper-V ou Clusters de Failover do Windows Server 2012 R2 executando a função Servidor de Arquivos de Escalabilidade Horizontal. confira [Atualização sem interrupção do sistema operacional do cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md).|
|Serviços de impressão e fax|    Não|    Não|    Sim (Printbrm.exe)|    Não|
|Serviços da área de trabalho Remota|    Sim, para todas as subfunções, mas o modo de farm misto não é compatível|    Sim, para todas as subfunções, mas o modo de farm misto não é compatível|    Sim|    Não|
|Servidor Web (IIS)|    Sim|    Sim|    Sim|    Não|
|Experiência do Windows Server Essentials|    Sim|    N/D, novo recurso|    Sim|    Não|
|Windows Server Update Services|    Sim|    Sim|    Sim|    Não|
|Pastas de trabalho|    Sim|    Sim|    Sim|    Sim do cluster do WS 2012 R2 ao usar a [Atualização sem interrupção do sistema operacional do cluster](../failover-clustering/cluster-operating-system-rolling-upgrade.md).|
