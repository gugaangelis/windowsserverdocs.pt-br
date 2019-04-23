---
title: A replicação está em pausa para uma ou mais máquinas virtuais neste servidor
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 248b5fbdbfb54380e441d14cde6beaa9146ce800
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827767"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>A replicação está em pausa para uma ou mais máquinas virtuais neste servidor

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Operações|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*A replicação está em pausa para uma ou mais das máquinas virtuais. Enquanto a máquina virtual primária está em pausa, quaisquer alterações que ocorram serão acumuladas e serão enviadas para a máquina virtual de réplica depois que a replicação é reiniciada.*  
  
## <a name="impact"></a>Impacto  
*Desde que a replicação está em pausa, alterações acumuladas ocorrendo na máquina virtual primária serão consomem espaço em disco disponível no servidor primário. Depois que a replicação é retomada, pode haver um grandes picos de tráfego de rede para o servidor de réplica. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Confirme que a replicação pausa pretendida. Se a replicação foi pausada para conectividade de rede ou espaço em disco insuficiente endereço, retome a replicação, assim que esses problemas sejam resolvidos.*  
  


