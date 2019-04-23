---
title: Configurar os sistemas operacionais convidados para backups baseados em VSS habilitar instantâneos consistentes por aplicativo para réplica do Hyper-V
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 7638e996-d42d-47b8-a670-1e09e7183850
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: b4300dd4b7adc0cef8544215b5da62044a97301b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863887"
---
# <a name="configure-guest-operating-systems-for-vss-based-backups-to-enable-application-consistent-snapshots-for-hyper-v-replica"></a>Configurar os sistemas operacionais convidados para backups baseados em VSS habilitar instantâneos consistentes por aplicativo para réplica do Hyper-V

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Instantâneos consistentes por aplicativo requer que o Volume Shadow Copy Services (VSS) é habilitado e configurado nos sistemas operacionais convidados de máquinas virtuais que participam da replicação.*  
  
## <a name="impact"></a>Impacto  
*Mesmo se os instantâneos consistentes por aplicativo são especificados na configuração de replicação, Hyper-V não usará a menos que o VSS está configurado. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use a Conexão de máquina Virtual para instalar o integration services na máquina virtual.*  
  


