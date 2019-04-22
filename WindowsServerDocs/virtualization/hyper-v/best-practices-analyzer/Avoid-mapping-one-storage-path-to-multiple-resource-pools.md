---
title: Evite o mapeamento de um caminho de armazenamento para vários pools de recursos
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 7c012836309f722e55c28b2ddbe3d54de641b4af
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823957"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Evite o mapeamento de um caminho de armazenamento para vários pools de recursos

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Operações|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Um caminho de arquivo de armazenamento é mapeado para vários pools de recursos.*  
  
## <a name="impact"></a>**Impacto**  
*Para o tipo de pool de armazenamento especificada, os seguintes pools de pai e filho compartilham o mesmo caminho de armazenamento:*  
  
\<lista de pools de >  
  
## <a name="resolution"></a>**Resolução**  
*Use o Windows PowerShell para reconfigurar os pools de recursos de armazenamento, de modo que vários pools não usam o mesmo caminho de armazenamento.*  
  


