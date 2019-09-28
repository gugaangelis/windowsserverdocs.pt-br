---
title: Evite habilitar a qualidade de serviço de armazenamento ao usar um disco rígido virtual diferencial quando os discos rígidos virtuais pai e filho estiverem em volumes diferentes
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 716a32de2f9327e5eca38c470fa1b7c44150e9cb
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366442"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Evite habilitar a qualidade de serviço de armazenamento ao usar um disco rígido virtual diferencial quando os discos rígidos virtuais pai e filho estiverem em volumes diferentes

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Um disco rígido virtual diferencial com os discos rígidos virtuais pai e filho em volumes diferentes tem a qualidade de serviço de armazenamento habilitada.*  
  
## <a name="impact"></a>**Causa**  
a configuração de @no__t 0This pode resultar em um comportamento de qualidade de serviço de armazenamento inesperado para o disco rígido virtual diferencial, bem como outros discos rígidos virtuais nos volumes pai e filho. Isso afeta os seguintes discos rígidos virtuais: *  
  
\<list de discos rígidos virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Desabilite a qualidade de serviço de armazenamento nos discos rígidos virtuais referenciados ou execute uma migração de armazenamento para mover o disco rígido virtual pai e filho para o mesmo volume.*  
  


