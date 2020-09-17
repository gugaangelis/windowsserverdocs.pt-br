---
title: Para participar da replicação, os servidores em clusters de failover devem ter um agente de réplica do Hyper-V configurado
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
ms.date: 8/16/2016
ms.openlocfilehash: 7fdd6167561be4922540a0def1f91f70b519aa0f
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746121"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Para participar da replicação, os servidores em clusters de failover devem ter um agente de réplica do Hyper-V configurado

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Erro|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*Para clusters de failover, a réplica do Hyper-V requer o uso de um nome de agente de réplica do Hyper-V em vez de um nome de servidor individual.*

## <a name="impact"></a>Impacto
*Se a máquina virtual for movida para um nó de cluster de failover diferente, a replicação não poderá continuar.*

## <a name="resolution"></a>Resolução
*Use Gerenciador de Cluster de Failover para configurar o agente de réplica do Hyper-V. No Gerenciador do Hyper-V, verifique se a configuração de replicação usa o nome do agente de réplica do Hyper-V como o nome do servidor.*



