---
title: Mais de um adaptador de rede deve estar disponível
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 678c0161e97b8dd022bbf0037d9add5de0281f77
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59884597"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Mais de um adaptador de rede deve estar disponível

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Erro|  
|**categoria**|Configuração|  

Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.

## <a name="issue"></a>Problema  
  
*Este servidor está configurado com um adaptador de rede, que deve ser compartilhado por sistema operacional de gerenciamento e todas as máquinas virtuais que exigem acesso a uma rede física.*  
  
## <a name="impact"></a>Impacto  
  
*Desempenho de rede pode estar danificado no sistema operacional de gerenciamento.*  
  
## <a name="resolution"></a>Resolução  
  
*Adicione mais adaptadores de rede a este computador. Para reservar um adaptador de rede para uso exclusivo por sistema operacional de gerenciamento, não configurá-lo para uso com uma rede virtual externa.*  
  
Para obter informações sobre como adicionar um adaptador de rede no computador, consulte a documentação para o computador ou o adaptador de rede. Em seguida, para reservá-lo exclusivamente para o sistema operacional de gerenciamento, não a conecte a um comutador virtual.   
  


