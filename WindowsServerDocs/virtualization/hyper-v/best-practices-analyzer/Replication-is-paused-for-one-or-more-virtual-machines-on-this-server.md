---
title: A replicação está em pausa para uma ou mais máquinas virtuais neste servidor
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 6974016dd564cf19d39a6d83b041020a4e327d2c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861809"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>A replicação está em pausa para uma ou mais máquinas virtuais neste servidor

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*A replicação está em pausa para uma ou mais máquinas virtuais. Enquanto a máquina virtual primária estiver pausada, as alterações que ocorrerem serão acumuladas e serão enviadas para a máquina virtual de réplica depois que a replicação for retomada.*  
  
## <a name="impact"></a>Impacto  
*Desde que a replicação seja pausada, as alterações acumuladas que ocorrerem na máquina virtual primária consumirão o espaço em disco disponível no servidor primário. Depois que a replicação é retomada, pode haver uma grande intermitência de tráfego de rede para o servidor de réplica. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Confirme se a pausa da replicação foi intencional. Se a replicação foi pausada para resolver o espaço em disco ou a conectividade de rede insuficiente, retome a replicação assim que esses problemas forem resolvidos.*  
  


