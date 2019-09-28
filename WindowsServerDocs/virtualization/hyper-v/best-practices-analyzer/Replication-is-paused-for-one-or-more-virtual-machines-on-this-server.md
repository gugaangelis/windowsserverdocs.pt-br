---
title: A replicação está em pausa para uma ou mais máquinas virtuais neste servidor
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 17d50f116c6cee488367c924bfbce3791a8d879f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393537"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>A replicação está em pausa para uma ou mais máquinas virtuais neste servidor

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
o *Replication está em pausa para uma ou mais máquinas virtuais. Enquanto a máquina virtual primária estiver em pausa, as alterações que ocorrerem serão acumuladas e serão enviadas para a máquina virtual de réplica depois que a replicação for retomada.*  
  
## <a name="impact"></a>Impacto  
*As tempo que a replicação é pausada, as alterações acumuladas que ocorrem na máquina virtual primária consumirão o espaço em disco disponível no servidor primário. Depois que a replicação é retomada, pode haver uma grande intermitência de tráfego de rede para o servidor de réplica. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Confirm que pausar a replicação foi pretendido. Se a replicação foi pausada para resolver o espaço em disco ou a conectividade de rede insuficiente, retome a replicação assim que esses problemas forem resolvidos.*  
  


