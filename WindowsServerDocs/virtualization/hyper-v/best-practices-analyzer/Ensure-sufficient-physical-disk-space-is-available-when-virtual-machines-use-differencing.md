---
title: Verifique se há espaço em disco físico suficiente disponível quando as máquinas virtuais usam discos rígidos virtuais diferenciais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 52e09cfb8695389d2c37def2c39ff43b4091fb76
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861949"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Verifique se há espaço em disco físico suficiente disponível quando as máquinas virtuais usam discos rígidos virtuais diferenciais

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Uma ou mais máquinas virtuais estão usando discos rígidos virtuais diferenciais.*  
  
## <a name="impact"></a>Impacto  
*Os discos rígidos virtuais diferenciais exigem o espaço disponível no volume de hospedagem para que o espaço possa ser alocado quando ocorrerem gravações nos discos rígidos virtuais. Se o espaço disponível for esgotado, qualquer máquina virtual que dependa do armazenamento físico poderá ser afetada. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Monitore o espaço em disco disponível para garantir que haja espaço suficiente disponível para a expansão do disco rígido virtual. Considere mesclar discos rígidos virtuais diferenciais em seu pai. No Gerenciador do Hyper-V, inspecione o disco diferencial para determinar o disco rígido virtual pai. Se você mesclar um disco diferencial a um disco pai que é compartilhado por outros discos diferenciais, essa ação corromperá a relação entre os outros discos diferenciais e o disco pai, tornando-os inutilizáveis. Depois de verificar se o disco rígido virtual pai não está compartilhado, você pode usar o assistente para editar disco para mesclar o disco diferencial para o disco rígido virtual pai.*  
  


