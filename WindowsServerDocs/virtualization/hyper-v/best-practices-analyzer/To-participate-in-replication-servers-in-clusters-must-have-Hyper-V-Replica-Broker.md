---
title: Para participar da replicação, os servidores em clusters de failover devem ter um agente de réplica do Hyper-V configurado
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2e15d2c4a467807397ef4712d2df1730b40d8024
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364601"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Para participar da replicação, os servidores em clusters de failover devem ter um agente de réplica do Hyper-V configurado

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Erro|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Para clusters de failover, a réplica do Hyper-V requer o uso de um nome de agente de réplica do Hyper-V em vez de um nome de servidor individual.*  
  
## <a name="impact"></a>Impacto  
*Se a máquina virtual for movida para um nó de cluster de failover diferente, a replicação não poderá continuar.*  
  
## <a name="resolution"></a>Resolução  
*Use Gerenciador de Cluster de Failover para configurar o agente de réplica do Hyper-V. No Gerenciador do Hyper-V, verifique se a configuração de replicação usa o nome do agente de réplica do Hyper-V como o nome do servidor.*  
  


