---
title: Portas seriais não devem ser configuradas em máquinas virtuais de geração 2
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 58c3fc5f975b85ce17ac5f7cca4930ec9e851e07
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877377"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>Portas seriais não devem ser configuradas em máquinas virtuais de geração 2

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Geração de um ou mais 2 máquinas virtuais tem uma porta serial configurada.*  
  
## <a name="impact"></a>**Impacto**  
*Desempenho poderá ser afetado para as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Se isso é intencional, nenhuma ação adicional é necessária. Caso contrário, considere usar Gerenciador do Hyper-V ou o Windows PowerShell para remover a cadeia de caracteres de conexão de portas seriais na máquina virtual.*  
  


