---
title: Autenticação baseada em certificado estiver configurada, mas o certificado especificado não está instalado no servidor de réplica ou em nós de cluster de failover
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4cabbce3-9367-4ddc-a108-1e5e1ab2bcff
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: feef30d2f798057e4bd8e53ebab240af6b1d25b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832937"
---
# <a name="certificate-based-authentication-is-configured-but-the-specified-certificate-is-not-installed-on-the-replica-server-or-failover-cluster-nodes"></a>Autenticação baseada em certificado estiver configurada, mas o certificado especificado não está instalado no servidor de réplica ou em nós de cluster de failover

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre as práticas recomendadas e varreduras, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  

Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.

## <a name="issue"></a>Problema  
  
*O certificado de segurança que a réplica do Hyper-V foi configurada para usar para fornecer replicação baseada em certificado não está instalada no servidor de réplica (ou quaisquer nós de cluster de failover).*  
  
## <a name="impact"></a>Impacto  
  
*No caso de um cluster de failover ou move para outro nó, a replicação do Hyper-V pausará se o novo nó não tiver o certificado apropriado instalado também. Isso afeta os seguintes nós:*  
  
\<list of nodes>  
  
## <a name="resolution"></a>Resolução  
  
*Instale o certificado configurado no servidor de réplica (e todos os respectivos nós no cluster de failover, se houver).*  
  


