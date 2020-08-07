---
title: Evite habilitar a qualidade de serviço de armazenamento ao usar um disco rígido virtual diferencial quando os discos rígidos virtuais pai e filho estiverem em volumes diferentes
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: b208a7a10679804666ff41a02d4cddbb4d8c6762
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87939178"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Evite habilitar a qualidade de serviço de armazenamento ao usar um disco rígido virtual diferencial quando os discos rígidos virtuais pai e filho estiverem em volumes diferentes

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
*Um disco rígido virtual diferencial com os discos rígidos virtuais pai e filho em volumes diferentes tem a qualidade de serviço de armazenamento habilitada.*

## <a name="impact"></a>**Impacto**
*Essa configuração pode resultar em um comportamento de qualidade de serviço de armazenamento inesperado para o disco rígido virtual diferencial, bem como outros discos rígidos virtuais nos volumes pai e filho. Isso afeta os seguintes discos rígidos virtuais:*

\<list of virtual hard disks>

## <a name="resolution"></a>**Resolução**
*Desabilite a qualidade de serviço de armazenamento nos discos rígidos virtuais referenciados ou execute uma migração de armazenamento para mover o disco rígido virtual pai e filho para o mesmo volume.*



