---
title: Configurar uma política para limitar o tráfego de replicação na rede
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5b3afd594f56973007a2f0f4318de8a8c7a98209
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365107"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurar uma política para limitar o tráfego de replicação na rede

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Pode não haver um limite para a quantidade de largura de banda de rede que a replicação pode consumir.*  
  
## <a name="impact"></a>Impacto  
a largura de banda *Network poderia se tornar totalmente dominada pelo tráfego de replicação, afetando outras atividades de rede críticas. Isso afeta as seguintes portas:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*If usar outro método para limitar o tráfego de rede, você poderá ignorá-lo. Caso contrário, use o editor de Política de Grupo para configurar uma política que limitará o tráfego de rede à porta relevante do servidor de réplica.*  
  
  


