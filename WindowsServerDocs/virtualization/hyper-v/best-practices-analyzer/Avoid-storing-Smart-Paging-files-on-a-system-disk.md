---
title: Evite armazenar arquivos de paginação inteligente em um disco do sistema
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9b57c9b8-76c5-43c7-bfa6-2c95b3cb6510
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: e3ddb662d14545693e26eb680527d93eb65d5d13
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365245"
---
# <a name="avoid-storing-smart-paging-files-on-a-system-disk"></a>Evite armazenar arquivos de paginação inteligente em um disco do sistema

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*A configuração de memória para uma ou mais máquinas virtuais pode exigir o uso de paginação inteligente se a máquina virtual for reinicializada e o local especificado para o arquivo de paginação inteligente é o disco do sistema do servidor que executa o Hyper-V.*  
  
## <a name="impact"></a>Impacto  
*Use do disco do sistema para paginação inteligente pode fazer com que o servidor que executa o Hyper-V tenha problemas. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Reconfigure as máquinas virtuais para armazenar os arquivos de paginação inteligente em um disco que não seja do sistema.*  
  


