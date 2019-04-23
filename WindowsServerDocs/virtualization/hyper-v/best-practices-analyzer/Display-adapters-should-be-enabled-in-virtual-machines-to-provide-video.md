---
title: Adaptadores de vídeo devem ser habilitadas em máquinas virtuais para fornecer recursos de vídeo
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d61461db471a876ddf46c1e5fec6992ffa80373
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870687"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Adaptadores de vídeo devem ser habilitadas em máquinas virtuais para fornecer recursos de vídeo

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre as práticas recomendadas e varreduras, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*O dispositivo de vídeo de barramento de máquina Virtual Microsoft podem ser desabilitado em uma máquina virtual.*  
  
Dispositivo de vídeo do Microsoft Virtual Machine barramento é um adaptador de vídeo virtual otimizado para uso com máquinas virtuais Hyper-V. Quando uma máquina virtual não está configurada para usar o dispositivo de vídeo de barramento de máquina Virtual Microsoft, um adaptador de vídeo herdado é usado. Dispositivo de vídeo do Microsoft Virtual Machine barramento é melhor do que um adaptador de vídeo herdado.  
  
## <a name="impact"></a>Impacto  
  
*Desempenho do vídeo para as seguintes máquinas virtuais será degradado:*  
  
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Use o Gerenciador de dispositivos no sistema operacional convidado para habilitar o dispositivo de vídeo de barramento de máquina Virtual Microsoft.*  
  
As etapas necessárias para usar o Gerenciador de dispositivos variam dependendo do sistema operacional. Para obter instruções, consulte a Ajuda no sistema operacional convidado.  
  


