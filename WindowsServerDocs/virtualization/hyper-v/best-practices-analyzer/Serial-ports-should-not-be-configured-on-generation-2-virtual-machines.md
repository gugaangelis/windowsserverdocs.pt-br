---
title: As portas seriais não devem ser configuradas em máquinas virtuais de geração 2
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8a8c15076921efa0e1e791a18c6a45ea1bf27b0e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364731"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>As portas seriais não devem ser configuradas em máquinas virtuais de geração 2

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
*Uma ou mais máquinas virtuais de geração 2 têm uma porta serial configurada.*  
  
## <a name="impact"></a>**Causa**  
*O desempenho pode ser afetado pelas seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*If isso é intencional, nenhuma ação adicional é necessária. Caso contrário, considere o uso do Gerenciador do Hyper-V ou do Windows PowerShell para remover a cadeia de conexão das portas seriais na máquina virtual.*  
  


