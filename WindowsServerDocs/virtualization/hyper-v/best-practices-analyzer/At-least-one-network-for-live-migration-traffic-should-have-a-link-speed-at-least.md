---
title: Pelo menos uma rede para o tráfego da migração ao vivo deve ter uma velocidade de link de pelo menos 1 Gbps
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 5714df3f-f810-4618-8c93-e24881651100
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: ef75f73bd934b863b146e93f4cdc7323e5d4c6fb
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837727"
---
# <a name="at-least-one-network-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Pelo menos uma rede para o tráfego da migração ao vivo deve ter uma velocidade de link de pelo menos 1 Gbps

>Aplica-se a: Windows Server 2016


  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Nenhuma das redes para o tráfego da migração ao vivo tem uma velocidade de link de pelo menos 1 Gbps.*  
  
## <a name="impact"></a>Impacto  
*As migrações ao vivo podem ocorrer lentamente, o que pode interromper a conexão de rede devido a um tempo limite da conexão TCP.*  
  
## <a name="resolution"></a>Resolução  
*Configure pelo menos uma rede de migração ao vivo com uma velocidade de 1 Gbps ou superior.*  
  
Consulte a documentação do seu fornecedor de hardware de rede para descobrir se qualquer um dos adaptadores de rede existente pode dar suporte a uma velocidade de link de pelo menos 1 Gbps.  
  


