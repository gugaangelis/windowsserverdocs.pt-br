---
title: A ressincronização da replicação deve ser agendada para o horário de pico
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 379f8c8cd6744fe5db176efb55a84f231ce45857
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71393514"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>A ressincronização da replicação deve ser agendada para o horário de pico

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*A ressincronização da replicação para as máquinas virtuais primárias não está agendada para horários de pico.*  
  
## <a name="impact"></a>Impacto  
*The mais uma máquina virtual está em um estado que requer ressincronização, quanto mais tempo os arquivos de log de replicação crescem e mais alterações não replicadas ocorrem nas máquinas virtuais primárias. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para modificar as configurações de replicação da máquina virtual para executar a ressincronização automaticamente fora do horário de pico.*  
  


