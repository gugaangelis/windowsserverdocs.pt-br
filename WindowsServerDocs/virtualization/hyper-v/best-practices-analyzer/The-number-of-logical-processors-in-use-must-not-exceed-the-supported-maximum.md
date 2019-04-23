---
title: O número de processadores lógicos em uso não deve exceder o máximo com suporte
description: Fornece instruções para resolver o problema relatado por essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 66df8b02-91d1-424b-8934-a39c214d530e
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: d1275a17cc04494708f5ecfe9b708834b4233641
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59847067"
---
# <a name="the-number-of-logical-processors-in-use-must-not-exceed-the-supported-maximum"></a>O número de processadores lógicos em uso não deve exceder o máximo com suporte

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Política|  
  
Nas seções a seguir, itálico indica o texto que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*O servidor está configurado com um número excessivo de processadores lógicos.*  
  
## <a name="impact"></a>Impacto  
  
*Microsoft não oferece suporte a Hyper-V em execução neste computador.*  
  
## <a name="resolution"></a>Resolução  
  
*Remova alguns processadores neste computador ou use o msconfig para limitar o número de processadores disponíveis.*  
  
Consulte as instruções a seguir para usar o Msconfig. Para obter detalhes sobre a remoção de processadores, consulte as instruções que acompanham o computador ou contate o fabricante do hardware. Para obter detalhes sobre configurações com suporte máximo para o Hyper-V, consulte [planejar a escalabilidade do Hyper-V no Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
### <a name="to-limit-the-number-of-available-processors"></a>Para limitar o número de processadores disponíveis  
  
1.  Abra o sistema de configuração de aplicativo (Msconfig.exe). Para fazer isso, clique em **inicie**, tipo **msconfig**, clique com botão direito a **configuração do sistema** aplicativo da área de trabalho e clique em **executar como administrador**.  
  
2.  Dos **Boot** , clique em **opções avançadas**.  
  
3.  Selecione **número de processadores** e, em seguida, selecione um número na lista. Clique em **OK**.  
  
4.  Reinicie o computador para executá-lo usando o novo número de processadores.  
  


