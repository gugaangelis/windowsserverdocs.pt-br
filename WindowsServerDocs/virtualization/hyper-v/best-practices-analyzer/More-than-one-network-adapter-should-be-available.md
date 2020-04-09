---
title: Mais de um adaptador de rede deve estar disponível
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 59940e56-e06a-490f-90ea-cf30d9f80b09
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 56cb747ac44d48b115dbf105ea96e4623d458b28
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80861889"
---
# <a name="more-than-one-network-adapter-should-be-available"></a>Mais de um adaptador de rede deve estar disponível

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Error|  
|**Categoria**|Configuração|  

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema  
  
*Esse servidor é configurado com um adaptador de rede, que deve ser compartilhado pelo sistema operacional de gerenciamento e por todas as máquinas virtuais que exigem acesso a uma rede física.*  
  
## <a name="impact"></a>Impacto  
  
*O desempenho da rede pode ser degradado no sistema operacional de gerenciamento.*  
  
## <a name="resolution"></a>Resolução  
  
*Adicione mais adaptadores de rede a este computador. Para reservar um adaptador de rede para uso exclusivo pelo sistema operacional de gerenciamento, não o configure para uso com uma rede virtual externa.*  
  
Para obter informações sobre como adicionar um adaptador de rede ao computador, consulte a documentação do computador ou adaptador de rede. Em seguida, para reservar exclusivamente para o sistema operacional de gerenciamento, não conecte-o a um comutador virtual.   
  


