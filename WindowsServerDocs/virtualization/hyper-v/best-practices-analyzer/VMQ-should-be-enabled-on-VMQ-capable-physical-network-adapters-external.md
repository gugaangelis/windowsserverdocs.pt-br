---
title: VMQ deve ser habilitado nos adaptadores de rede física compatível com VMQ associados a um comutador virtual externo
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 9a552c15675e6ca7a7310c8c9eaec883653987be
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889467"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>VMQ deve ser habilitado nos adaptadores de rede física compatível com VMQ associados a um comutador virtual externo

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Os seguintes adaptadores de rede são capazes de fila de máquina virtual (VMQ), mas o recurso está desabilitado.*  
  
## <a name="impact"></a>**Impacto**  
*Windows não não capaz de aproveitar ao máximo os descarregamentos de hardware disponível nos seguintes adaptadores de rede:*  
  
\<lista de adaptadores de rede >  
  
## <a name="resolution"></a>**Resolução**  
*Habilite VMQ, usando o cmdlet Enable-NetAdapterVmq Windows PowerShell ou usando a interface de usuário de propriedades avançadas do adaptador de rede.*  
  


