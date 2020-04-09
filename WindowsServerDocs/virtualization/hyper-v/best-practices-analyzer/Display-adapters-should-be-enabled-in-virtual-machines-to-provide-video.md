---
title: Os adaptadores de vídeo devem ser habilitados em máquinas virtuais para fornecer recursos de vídeo
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1821aae18b1712ba7d839ca9f42318edc5ef8a35
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861999"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Os adaptadores de vídeo devem ser habilitados em máquinas virtuais para fornecer recursos de vídeo

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre práticas recomendadas e verificações, consulte* [analisador de práticas recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*O dispositivo de vídeo do barramento de máquina virtual da Microsoft pode estar desabilitado em uma máquina virtual.*  
  
O dispositivo de vídeo do barramento de máquina virtual da Microsoft é um adaptador de vídeo virtual otimizado para uso com máquinas virtuais do Hyper-V. Quando uma máquina virtual não está configurada para usar o dispositivo de vídeo do barramento de máquina virtual da Microsoft, é usado um adaptador de vídeo herdado. O dispositivo de vídeo do barramento de máquina virtual da Microsoft executa melhor do que um adaptador de vídeo herdado.  
  
## <a name="impact"></a>Impacto  
  
*O desempenho de vídeo para as seguintes máquinas virtuais será degradado:*  
  
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Use Device Manager no sistema operacional convidado para habilitar o dispositivo de vídeo do barramento de máquina virtual da Microsoft.*  
  
As etapas necessárias para usar Device Manager variam de acordo com o sistema operacional. Para obter instruções, consulte a ajuda no sistema operacional convidado.  
  


