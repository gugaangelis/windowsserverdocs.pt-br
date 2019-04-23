---
title: Certifique-se de que o driver de função virtual funcione corretamente quando uma máquina virtual é configurada para usar SR-IOV
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8d3d0a5008b55d4823cef9a8dd2a7bce4a6a2a33
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59852077"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>Certifique-se de que o driver de função virtual funcione corretamente quando uma máquina virtual é configurada para usar SR-IOV

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
*O driver de função virtual não está funcionando corretamente no sistema operacional convidado de uma ou mais máquinas virtuais.*  
  
## <a name="impact"></a>Impacto  
*Desempenho de rede não é ideal nas seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*No sistema operacional convidado, faça o seguinte: Verifique se os drivers adequados estão instalados e todos os dispositivos de rede estão habilitados e verifique o log de eventos para erros ou avisos.*  
  


