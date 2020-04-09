---
title: Evite mapear um caminho de armazenamento para vários pools de recursos
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 53ef04a2dde875b26dd109075a2cfa4484ebd5cd
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857749"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Evite mapear um caminho de armazenamento para vários pools de recursos

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Um caminho de arquivo de armazenamento é mapeado para vários pools de recursos.*  
  
## <a name="impact"></a>**Causa**  
*Para o tipo de pool de armazenamento especificado, os seguintes pools pai e filho compartilham o mesmo caminho de armazenamento:*  
  
\<lista de pools >  
  
## <a name="resolution"></a>**Resolução**  
*Use o Windows PowerShell para reconfigurar os pools de recursos de armazenamento para que vários pools não usem o mesmo caminho de armazenamento.*  
  


