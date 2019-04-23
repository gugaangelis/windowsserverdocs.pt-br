---
title: Instantâneos de recuperação devem ser removidos após o failover
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4663320df91019fc7dc1d8ca7ffdb2fcc3e0de42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837677"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Instantâneos de recuperação devem ser removidos após o failover

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
*Um failover de máquina virtual tem um ou mais instantâneos de recuperação.*  
  
## <a name="impact"></a>**Impacto**  
*Espaço disponível pode executá-los no disco físico que armazena os arquivos de instantâneo. Se isso ocorrer, nenhuma operação de disco adicional pode ser executada no armazenamento físico. Qualquer máquina virtual que se baseia no armazenamento físico pode ser afetada. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Para cada um failover de máquina virtual, use o cmdlet Complete-VMFailover no Windows PowerShell para remover os instantâneos de recuperação e indicar a conclusão do failover.*  
  


