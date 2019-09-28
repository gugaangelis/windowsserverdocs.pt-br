---
title: Evite mapear um caminho de armazenamento para vários pools de recursos
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e89c0382d20d586d8c0b50396ddbd56d6fdadf0b
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365259"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Evite mapear um caminho de armazenamento para vários pools de recursos

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Um caminho de arquivo de armazenamento é mapeado para vários pools de recursos.*  
  
## <a name="impact"></a>**Causa**  
*Para o tipo de pool de armazenamento especificado, os seguintes pools pai e filho compartilham o mesmo caminho de armazenamento:*  
  
\<list de pools >  
  
## <a name="resolution"></a>**Resolução**  
*Use o Windows PowerShell para reconfigurar os pools de recursos de armazenamento para que vários pools não usem o mesmo caminho de armazenamento.*  
  


