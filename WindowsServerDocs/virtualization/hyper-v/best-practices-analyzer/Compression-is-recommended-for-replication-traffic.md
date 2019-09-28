---
title: A compactação é recomendada para o tráfego de replicação
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 77a314e816c36f626ea3edb10b80f65e3897e7c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365101"
---
# <a name="compression-is-recommended-for-replication-traffic"></a>A compactação é recomendada para o tráfego de replicação

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
*O tráfego de replicação enviado pela rede do servidor primário para o servidor de réplica é descompactado.*  
  
## <a name="impact"></a>Impacto  
o tráfego de @no__t 0Replication usará mais largura de banda do que o necessário. Isso afeta as seguintes máquinas virtuais: *  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
Réplica do Hyper-V de @no__t 0Configure para compactar os dados transmitidos pela rede nas configurações da máquina virtual no Gerenciador do Hyper-V. Você também pode usar ferramentas fora do Hyper-V para executar a compactação. *  
  


