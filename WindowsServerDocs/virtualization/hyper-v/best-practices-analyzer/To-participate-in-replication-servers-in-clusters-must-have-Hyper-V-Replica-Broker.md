---
title: Para participar da replicação, servidores em clusters de failover devem ter um agente de réplica do Hyper-V configurado
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5ec88ce5-a8b2-4ece-9062-366523c8b17f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d4966396af955f9c8bad34b5b2892115e93c3b85
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59887967"
---
# <a name="to-participate-in-replication-servers-in-failover-clusters-must-have-a-hyper-v-replica-broker-configured"></a>Para participar da replicação, servidores em clusters de failover devem ter um agente de réplica do Hyper-V configurado

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Para clusters de failover, a réplica do Hyper-V exige o uso de um nome de agente de réplica do Hyper-V em vez de um nome de servidor individuais.*  
  
## <a name="impact"></a>Impacto  
*Se a máquina virtual é movida para um nó de cluster de failover diferentes, a replicação não pode continuar.*  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador de Cluster de Failover para configurar o agente de réplica do Hyper-V. No Gerenciador do Hyper-V, certifique-se de que a configuração de replicação usa o nome do agente de réplica do Hyper-V como o nome do servidor.*  
  


