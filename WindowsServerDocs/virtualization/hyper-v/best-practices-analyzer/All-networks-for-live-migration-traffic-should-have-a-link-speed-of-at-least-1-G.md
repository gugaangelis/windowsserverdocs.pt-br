---
title: Todas as redes para tráfego de migração ao vivo devem ter uma velocidade de link de pelo menos 1 Gbps
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 89411b63-bec8-463d-b486-107548ed440e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: aa341f1e1aed05684020fa1028902839bc1e412d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948609"
---
# <a name="all-networks-for-live-migration-traffic-should-have-a-link-speed-of-at-least-1-gbps"></a>Todas as redes para tráfego de migração ao vivo devem ter uma velocidade de link de pelo menos 1 Gbps

>Aplica-se a: Windows Server 2016



|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*Nenhuma das redes para o tráfego de migração ao vivo tem uma velocidade de link de pelo menos 1 Gbps.*

## <a name="impact"></a>Impacto
*As migrações ao vivo podem ocorrer lentamente, o que pode interromper a conexão de rede devido a um tempo limite de conexão TCP.*

## <a name="resolution"></a>Resolução
*Configure pelo menos uma rede de migração dinâmica com velocidade de 1 Gbps ou mais rápida.*

Consulte a documentação do fornecedor de hardware de rede para saber se algum adaptador de rede existente pode dar suporte a uma velocidade de link de pelo menos 1 Gbps.



