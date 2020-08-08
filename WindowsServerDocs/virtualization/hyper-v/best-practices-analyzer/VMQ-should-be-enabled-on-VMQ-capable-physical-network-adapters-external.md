---
title: A VMQ deve ser habilitada em adaptadores de rede física compatíveis com VMQ vinculados a um comutador virtual externo
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 93d1b155-bf44-46b0-bb69-d34d5b30e574
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ae8c5005f9038ac2b82fd3f56016da357a4a9364
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960229"
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



