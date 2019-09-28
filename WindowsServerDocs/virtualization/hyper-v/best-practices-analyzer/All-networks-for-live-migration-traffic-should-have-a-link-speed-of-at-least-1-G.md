---
title: Todas as redes para tráfego de migração ao vivo devem ter uma velocidade de link de pelo menos 1 Gbps
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 89411b63-bec8-463d-b486-107548ed440e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 92ba74ec75d8e90979e1cc329415a52af0218f54
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365296"
---
# <a name="all-networks-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Todas as redes para tráfego de migração ao vivo devem ter uma velocidade de link de pelo menos 1 Gbps

>Aplica-se a: Windows Server 2016


  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Nenhuma das redes para o tráfego de migração ao vivo tem uma velocidade de link de pelo menos 1 Gbps.*  
  
## <a name="impact"></a>Impacto  
*As migrações ao vivo podem ocorrer lentamente, o que pode interromper a conexão de rede devido a um tempo limite de conexão TCP.*  
  
## <a name="resolution"></a>Resolução  
*Configure pelo menos uma rede de migração dinâmica com velocidade de 1 Gbps ou mais rápida.*  
  
Consulte a documentação do fornecedor de hardware de rede para saber se algum adaptador de rede existente pode dar suporte a uma velocidade de link de pelo menos 1 Gbps.  
  


