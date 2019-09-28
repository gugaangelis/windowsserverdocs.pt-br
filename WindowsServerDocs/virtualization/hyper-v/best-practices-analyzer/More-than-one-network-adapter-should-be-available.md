---
title: Mais de um adaptador de rede deve estar disponível
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6b043900c6fde4522e5805a1f0c1a635de335e31
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364800"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Mais de um adaptador de rede deve estar disponível

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Erro|  
|**Categorias**|Configuração|  

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema  
  
*Esse servidor é configurado com um adaptador de rede, que deve ser compartilhado pelo sistema operacional de gerenciamento e por todas as máquinas virtuais que exigem acesso a uma rede física.*  
  
## <a name="impact"></a>Impacto  
  
*O desempenho da rede pode ser degradado no sistema operacional de gerenciamento.*  
  
## <a name="resolution"></a>Resolução  
  
*Add mais adaptadores de rede para este computador. Para reservar um adaptador de rede para uso exclusivo pelo sistema operacional de gerenciamento, não o configure para uso com uma rede virtual externa.*  
  
Para obter informações sobre como adicionar um adaptador de rede ao computador, consulte a documentação do computador ou adaptador de rede. Em seguida, para reservar exclusivamente para o sistema operacional de gerenciamento, não conecte-o a um comutador virtual.   
  


