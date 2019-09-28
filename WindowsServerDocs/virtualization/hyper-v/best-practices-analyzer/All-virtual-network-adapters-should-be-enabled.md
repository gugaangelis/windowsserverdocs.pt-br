---
title: Todos os adaptadores de rede virtual devem ser habilitados
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: b17d647d-a34a-44de-ada6-01a2bf5eeb48
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: fce564fdb47d0677b36078f3d8446579bc06816c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366592"
---
# <a name="all-virtual-network-adapters-should-be-enabled"></a>Todos os adaptadores de rede virtual devem ser habilitados

>Aplica-se a: Windows Server 2016


  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Um ou mais adaptadores de rede virtual associados a um adaptador de rede física estão desabilitados no sistema operacional de gerenciamento.*  
  
## <a name="impact"></a>Impacto  
  
*A configuração deste servidor não é a ideal.*  
  
O sistema operacional de gerenciamento não pode se conectar a uma rede física (externa) usando um dos adaptadores de rede física neste computador porque ele está associado a um adaptador de rede virtual desabilitado.  
  
## <a name="resolution"></a>Resolução  
  
*Use rede & configurações da Internet para habilitar o adaptador de rede virtual. Ou use o Gerenciador de comutador virtual para reconfigurar o comutador virtual externo para que ele não seja compartilhado com o sistema operacional de gerenciamento.*  
  


