---
title: A VMQ deve ser habilitada em adaptadores de rede física compatíveis com VMQ vinculados a um comutador virtual externo
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a66c1c4580f8ecda90caa5446e74bc9b12ac0476
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855039"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>A VMQ deve ser habilitada em adaptadores de rede física compatíveis com VMQ vinculados a um comutador virtual externo

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>**Problema**  
*Os adaptadores de rede a seguir são capazes de uma VMQ (fila de máquina virtual), mas o recurso está desabilitado.*  
  
## <a name="impact"></a>**Causa**  
*O Windows não pode aproveitar ao máximo os descarregamentos de hardware disponíveis nos seguintes adaptadores de rede:*  
  
\<lista de adaptadores de rede >  
  
## <a name="resolution"></a>**Resolução**  
*Habilite a VMQ usando o cmdlet Enable-NetAdapterVmq do Windows PowerShell ou usando a interface do usuário de propriedades avançadas para o adaptador de rede.*  
  


