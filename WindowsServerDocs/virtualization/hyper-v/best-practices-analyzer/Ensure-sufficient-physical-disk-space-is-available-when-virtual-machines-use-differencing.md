---
title: Certifique-se de espaço suficiente em disco físico está disponível quando máquinas virtuais usam discos rígidos virtuais diferentes
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 6d4d219402cdc321a75cc27d75ea7749eb6127e0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855567"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Certifique-se de espaço suficiente em disco físico está disponível quando máquinas virtuais usam discos rígidos virtuais diferentes

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Uma ou mais máquinas virtuais estão usando discos rígidos virtuais diferenciais.*  
  
## <a name="impact"></a>Impacto  
*Discos rígidos virtuais diferentes exigem espaço disponível no volume do host, de forma que pode ser alocado espaço quando ocorrem gravações em discos rígidos virtuais. Se o espaço disponível está esgotado, qualquer máquina virtual que se baseia no armazenamento físico pode ser afetada. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Monitorar espaço em disco disponível para garantir espaço suficiente disponível para expansão de disco rígido virtual. Considere mesclar os discos rígidos virtuais diferenciais em seu pai. No Gerenciador do Hyper-V, inspecione o disco diferencial para determinar o disco rígido virtual pai. Se você mesclar um disco diferencial em um disco pai que é compartilhado por outros discos diferenciais, essa ação irá corromper a relação entre os outros discos diferenciais e o disco pai, tornando-os inutilizáveis. Depois de verificar que o disco rígido virtual pai não é compartilhado, você pode usar o Assistente para edição de disco para mesclar o disco diferencial para o disco rígido virtual pai.*  
  


