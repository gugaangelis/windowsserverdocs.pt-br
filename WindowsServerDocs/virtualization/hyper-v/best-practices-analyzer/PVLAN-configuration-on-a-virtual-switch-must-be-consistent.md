---
title: A configuração do PVLAN em um comutador virtual deve ser consistente
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 7e42c874e883157b3f85f523511b950e2863b155
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861849"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>A configuração do PVLAN em um comutador virtual deve ser consistente

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016| 
|**Produto/recurso**|Hyper-V|  
|**Severity**|Error|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*A rede de área local virtual (PVLAN) privada não está configurada corretamente em um ou mais adaptadores de rede virtual.*  
  
## <a name="impact"></a>**Causa**  
*PVLAN pode não isolar o tráfego de rede entre máquinas virtuais corretamente.*  
  
## <a name="resolution"></a>**Resolução**  
*Use o cmdlet do Windows PowerShell, Set-VMNetworkAdapterVlan, para configurar o PVLAN corretamente.*  
  


