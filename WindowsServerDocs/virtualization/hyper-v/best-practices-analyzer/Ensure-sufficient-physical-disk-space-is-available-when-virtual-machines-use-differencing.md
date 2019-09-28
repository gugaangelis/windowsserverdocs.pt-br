---
title: Verifique se há espaço em disco físico suficiente disponível quando as máquinas virtuais usam discos rígidos virtuais diferenciais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3827d149d2d691b4ecd7fe6ae8f6d7255c85a31c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364874"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Verifique se há espaço em disco físico suficiente disponível quando as máquinas virtuais usam discos rígidos virtuais diferenciais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Uma ou mais máquinas virtuais estão usando discos rígidos virtuais diferenciais.*  
  
## <a name="impact"></a>Impacto  
os discos rígidos virtuais *Differencing exigem o espaço disponível no volume de hospedagem para que o espaço possa ser alocado quando as gravações nos discos rígidos virtuais ocorrerem. Se o espaço disponível for esgotado, qualquer máquina virtual que dependa do armazenamento físico poderá ser afetada. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Monitor espaço em disco disponível para garantir que haja espaço suficiente disponível para a expansão do disco rígido virtual. Considere mesclar discos rígidos virtuais diferenciais em seu pai. No Gerenciador do Hyper-V, inspecione o disco diferencial para determinar o disco rígido virtual pai. Se você mesclar um disco diferencial a um disco pai que é compartilhado por outros discos diferenciais, essa ação corromperá a relação entre os outros discos diferenciais e o disco pai, tornando-os inutilizáveis. Depois de verificar se o disco rígido virtual pai não está compartilhado, você pode usar o assistente para editar disco para mesclar o disco diferencial para o disco rígido virtual pai.*  
  


