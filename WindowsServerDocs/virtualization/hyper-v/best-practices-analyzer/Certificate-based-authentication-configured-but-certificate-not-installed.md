---
title: A autenticação baseada em certificado está configurada, mas o certificado especificado não está instalado no servidor de réplica ou nos nós de cluster de failover
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0b107a4760cc3470c7f80d53feef00a2f8f789c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365204"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>A autenticação baseada em certificado está configurada, mas o certificado especificado não está instalado no servidor de réplica ou nos nós de cluster de failover

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre práticas recomendadas e verificações, consulte* [analisador de práticas recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Erro|  
|**Categorias**|Configuração|  

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema  
  
*O certificado de segurança que a réplica do Hyper-V foi configurado para fornecer replicação baseada em certificado não está instalado no servidor de réplica (ou em qualquer nó de cluster de failover).*  
  
## <a name="impact"></a>Impacto  
  
*In o evento de um failover de cluster ou mover para outro nó, a replicação do Hyper-V será pausada se o novo nó também não tiver o certificado apropriado instalado. Isso afeta os seguintes nós:*  
  
\<list de nós >  
  
## <a name="resolution"></a>Resolução  
  
*Instale o certificado configurado no servidor de réplica (e todos os nós associados no cluster de failover, se houver).*  
  


