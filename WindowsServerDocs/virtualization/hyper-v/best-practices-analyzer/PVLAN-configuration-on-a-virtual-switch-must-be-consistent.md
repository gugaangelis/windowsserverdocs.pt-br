---
title: Configuração de PVLAN em um comutador virtual deve ser consistente
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b7d6d068027aa9497b00138bd1d889ea86aa3308
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59813247"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>Configuração de PVLAN em um comutador virtual deve ser consistente

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016| 
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Privada Virtual Local Area Network (PVLAN) não está configurado corretamente em um ou mais adaptadores de rede virtual.*  
  
## <a name="impact"></a>**Impacto**  
*PVLAN pode isolar o tráfego de rede entre máquinas virtuais corretamente.*  
  
## <a name="resolution"></a>**Resolução**  
*Use o cmdlet do Windows PowerShell, Set-VMNetworkAdapterVlan para configurar corretamente a PVLAN.*  
  


