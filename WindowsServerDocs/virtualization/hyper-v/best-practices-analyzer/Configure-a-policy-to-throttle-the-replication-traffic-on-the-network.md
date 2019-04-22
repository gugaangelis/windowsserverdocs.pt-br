---
title: Configurar uma política para restringir o tráfego de replicação na rede
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2c1f1865fa1d611c0b5baaf981140f9807b51458
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818687"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurar uma política para restringir o tráfego de replicação na rede

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Talvez não haja um limite na quantidade de largura de banda de rede que a replicação pode consumir.*  
  
## <a name="impact"></a>Impacto  
*Largura de banda de rede poderia se tornar completamente dominada por tráfego de replicação, que afetam a outra atividade de rede críticos. Isso afeta as seguintes portas:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Se você usar outro método para restringir o tráfego de rede, você pode ignorar isso. Caso contrário, use o Editor de diretiva de grupo para configurar uma política que limitará o tráfego de rede para a porta relevante do servidor de réplica.*  
  
  


