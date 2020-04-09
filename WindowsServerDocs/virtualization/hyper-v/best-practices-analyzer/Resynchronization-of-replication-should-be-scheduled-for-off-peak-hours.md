---
title: A ressincronização da replicação deve ser agendada para o horário de pico
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 093a7bb7-8e0a-486b-b42b-04edd8809710
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ae630f93ef50ebc977de2bffcefa3b27b03622ee
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861789"
---
# <a name="resynchronization-of-replication-should-be-scheduled-for-off-peak-hours"></a>A ressincronização da replicação deve ser agendada para o horário de pico

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
*A ressincronização da replicação para as máquinas virtuais primárias não está agendada para horários de pico.*  
  
## <a name="impact"></a>Impacto  
*Quanto mais longa uma máquina virtual estiver em um estado que exija ressincronização, mais tempo os arquivos de log de replicação aumentarão e mais alterações não replicadas ocorrerão nas máquinas virtuais primárias. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para modificar as configurações de replicação da máquina virtual para executar a ressincronização automaticamente fora do horário de pico.*  
  


