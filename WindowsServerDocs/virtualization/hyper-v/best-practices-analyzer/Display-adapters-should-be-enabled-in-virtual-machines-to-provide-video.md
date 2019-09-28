---
title: Os adaptadores de vídeo devem ser habilitados em máquinas virtuais para fornecer recursos de vídeo
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 0c515c7fb1ed160dfee1e1b7303022082e936157
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364913"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Os adaptadores de vídeo devem ser habilitados em máquinas virtuais para fornecer recursos de vídeo

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre práticas recomendadas e verificações, consulte* [analisador de práticas recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*O dispositivo de vídeo do barramento de máquina virtual da Microsoft pode estar desabilitado em uma máquina virtual.*  
  
O dispositivo de vídeo do barramento de máquina virtual da Microsoft é um adaptador de vídeo virtual otimizado para uso com máquinas virtuais do Hyper-V. Quando uma máquina virtual não está configurada para usar o dispositivo de vídeo do barramento de máquina virtual da Microsoft, é usado um adaptador de vídeo herdado. O dispositivo de vídeo do barramento de máquina virtual da Microsoft executa melhor do que um adaptador de vídeo herdado.  
  
## <a name="impact"></a>Impacto  
  
*O desempenho de vídeo para as seguintes máquinas virtuais será degradado:*  
  
\<list de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Use Device Manager no sistema operacional convidado para habilitar o dispositivo de vídeo do barramento de máquina virtual da Microsoft.*  
  
As etapas necessárias para usar Device Manager variam de acordo com o sistema operacional. Para obter instruções, consulte a ajuda no sistema operacional convidado.  
  


