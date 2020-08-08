---
title: A replicação está em pausa para uma ou mais máquinas virtuais neste servidor
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: e1119a40-eda3-4058-8648-7df81cbc6c29
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: a40c4d45eea6d0c363cd03d5eef94543ddc1317d
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948424"
---
# <a name="replication-is-paused-for-one-or-more-virtual-machines-on-this-server"></a>A replicação está em pausa para uma ou mais máquinas virtuais neste servidor

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Operações|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*A replicação está em pausa para uma ou mais máquinas virtuais. Enquanto a máquina virtual primária estiver pausada, as alterações que ocorrerem serão acumuladas e serão enviadas para a máquina virtual de réplica depois que a replicação for retomada.*

## <a name="impact"></a>Impacto
*Desde que a replicação seja pausada, as alterações acumuladas que ocorrerem na máquina virtual primária consumirão o espaço em disco disponível no servidor primário. Depois que a replicação é retomada, pode haver uma grande intermitência de tráfego de rede para o servidor de réplica. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Confirme se a pausa da replicação foi intencional. Se a replicação foi pausada para resolver o espaço em disco ou a conectividade de rede insuficiente, retome a replicação assim que esses problemas forem resolvidos.*



