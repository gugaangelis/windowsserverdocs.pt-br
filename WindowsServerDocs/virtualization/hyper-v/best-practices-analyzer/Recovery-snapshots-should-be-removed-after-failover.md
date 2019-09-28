---
title: Os instantâneos de recuperação devem ser removidos após o failover
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 922115fa-e8dd-4055-aaf1-4a4437c5cf28
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 4b8574956fb1b46ca0cf9678187fffcd68c2d261
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393530"
---
# <a name="recovery-snapshots-should-be-removed-after-failover"></a>Os instantâneos de recuperação devem ser removidos após o failover

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
*Uma máquina virtual com failover tem um ou mais instantâneos de recuperação.*  
  
## <a name="impact"></a>**Causa**  
o espaço de @no__t 0Available pode ficar no disco físico que armazena os arquivos de instantâneo. Se isso ocorrer, nenhuma operação de disco adicional poderá ser executada no armazenamento físico. Qualquer máquina virtual que dependa do armazenamento físico poderá ser afetada. Isso afeta as seguintes máquinas virtuais: *  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Para cada máquina virtual com failover, use o cmdlet Complete-VMFailover no Windows PowerShell para remover os instantâneos de recuperação e indicar a conclusão do failover.*  
  


