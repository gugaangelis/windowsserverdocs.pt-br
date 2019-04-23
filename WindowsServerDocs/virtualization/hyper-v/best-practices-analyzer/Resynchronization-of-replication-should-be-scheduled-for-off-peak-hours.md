---
title: A ressincronização da replicação deve ser agendada para o horário de pico
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2d6c18b7e37c5d17f56f41c7ff03ed8796457de0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840017"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>A ressincronização da replicação deve ser agendada para o horário de pico

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
*A ressincronização de replicação para máquinas virtuais primárias não está agendada para o horário de pico.*  
  
## <a name="impact"></a>Impacto  
*É mais uma máquina virtual em um estado que exigem ressincronização, crescem mais os arquivos de log de replicação e as alterações não replicadas mais ocorrem nas máquinas virtuais primárias. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para modificar as configurações de replicação para a máquina virtual para executar a ressincronização automaticamente durante o horário de pico.*  
  


