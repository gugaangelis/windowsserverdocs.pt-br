---
title: Todas as redes para tráfego de migração ao vivo devem ter uma velocidade de link de pelo menos 1 Gbps
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 89411b63-bec8-463d-b486-107548ed440e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: adb16b1c4618e0874f48f4715440a9d903f5bc9f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857839"
---
# <a name="all-networks-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Todas as redes para tráfego de migração ao vivo devem ter uma velocidade de link de pelo menos 1 Gbps

>Aplica-se a: Windows Server 2016


  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Nenhuma das redes para o tráfego de migração ao vivo tem uma velocidade de link de pelo menos 1 Gbps.*  
  
## <a name="impact"></a>Impacto  
*As migrações ao vivo podem ocorrer lentamente, o que pode interromper a conexão de rede devido a um tempo limite de conexão TCP.*  
  
## <a name="resolution"></a>Resolução  
*Configure pelo menos uma rede de migração dinâmica com velocidade de 1 Gbps ou mais rápida.*  
  
Consulte a documentação do fornecedor de hardware de rede para saber se algum adaptador de rede existente pode dar suporte a uma velocidade de link de pelo menos 1 Gbps.  
  


