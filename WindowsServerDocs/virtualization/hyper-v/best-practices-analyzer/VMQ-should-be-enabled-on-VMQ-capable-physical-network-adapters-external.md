---
title: A VMQ deve ser habilitada em adaptadores de rede física compatíveis com VMQ vinculados a um comutador virtual externo
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
ms.date: 8/16/2016
ms.openlocfilehash: 169f2ea06bf35c7bbc9bcaec354ca66e118c75d2
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746761"
---
# <a name="vmq-should-be-enabled-on-vmq-capable-physical-network-adapters-bound-to-an-external-virtual-switch"></a>A VMQ deve ser habilitada em adaptadores de rede física compatíveis com VMQ vinculados a um comutador virtual externo

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>**Problema**
*Os adaptadores de rede a seguir são capazes de uma VMQ (fila de máquina virtual), mas o recurso está desabilitado.*

## <a name="impact"></a>**Impacto**
*O Windows não pode aproveitar ao máximo os descarregamentos de hardware disponíveis nos seguintes adaptadores de rede:*

\<list of network adapters>

## <a name="resolution"></a>**Resolução**
*Habilite a VMQ usando o cmdlet Enable-NetAdapterVmq do Windows PowerShell ou usando a interface do usuário de propriedades avançadas para o adaptador de rede.*



