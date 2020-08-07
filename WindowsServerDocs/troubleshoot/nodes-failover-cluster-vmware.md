---
title: Remover nó da associação ao cluster de failover ativo
description: Este artigo aborda a questão de localizar nós removidos da Associação de cluster de failover ativo.
ms.date: 05/28/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: da46b39f853476676a06bcaaa20338dd1a178586
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87935566"
---
# <a name="nodes-being-removed-from-failover-cluster-membership-on-vmware-esx"></a>Nós sendo removidos da Associação de cluster de failover no VMWare ESX

Este artigo aborda a questão de localizar nós removidos da Associação de cluster de failover ativo quando os nós são hospedados no VMWare ESX.

## <a name="symptom"></a>Sintoma

Quando o problema ocorrer, você verá no log de eventos do sistema do Visualizador de Eventos:

![Evento 1135](media/nodes-failover-cluster-vmware/1135.png)

## <a name="resolution"></a>Resolução

Um problema específico é com os adaptadores VMXNET3 descartando pacotes de rede de entrada porque o buffer de entrada está definido muito baixo para lidar com grandes quantidades de tráfego. Podemos descobrir facilmente se esse é um problema usando o monitor de desempenho para examinar o contador "rede Interface\Packets recebida descartada".

![Adicionar Contadores](media/nodes-failover-cluster-vmware/0527.png)

Depois de adicionar esse contador, examine os números médio, mínimo e máximo e, se eles forem qualquer valor maior que zero, o buffer de recebimento precisará ser ajustado para o adaptador. Esse problema está documentado na base de dados de conhecimento da VMWare: [grande perda de pacotes no nível do SO convidado no VMXNET3 vNIC no ESXi 5. x/4. x](https://kb.vmware.com/s/article/2039495).
