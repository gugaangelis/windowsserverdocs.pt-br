---
title: Configurar uma política para limitar o tráfego de replicação na rede
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 82cb1aef-cdc3-4d0a-88d4-ef497ab79606
ms.date: 8/16/2016
ms.openlocfilehash: 18c1c1e586075dfb1ef477c1d3f21e549791464a
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746981"
---
# <a name="configure-a-policy-to-throttle-the-replication-traffic-on-the-network"></a>Configurar uma política para limitar o tráfego de replicação na rede

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*Pode não haver um limite para a quantidade de largura de banda de rede que a replicação pode consumir.*

## <a name="impact"></a>Impacto
*A largura de banda da rede poderia se tornar totalmente dominada pelo tráfego de replicação, afetando outras atividades de rede críticas. Isso afeta as seguintes portas:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Se você usar outro método para limitar o tráfego de rede, poderá ignorá-lo. Caso contrário, use o editor de Política de Grupo para configurar uma política que limitará o tráfego de rede à porta relevante do servidor de réplica.*




