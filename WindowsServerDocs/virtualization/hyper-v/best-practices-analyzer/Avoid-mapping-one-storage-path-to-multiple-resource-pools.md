---
title: Evite mapear um caminho de armazenamento para vários pools de recursos
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 24992453-762b-4892-9a50-55d237b9b7f2
ms.date: 8/16/2016
ms.openlocfilehash: fb2756889907dd9e268782816a9d035c9e6478d7
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747041"
---
# <a name="avoid-mapping-one-storage-path-to-multiple-resource-pools"></a>Evite mapear um caminho de armazenamento para vários pools de recursos

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Operações|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>**Problema**
*Um caminho de arquivo de armazenamento é mapeado para vários pools de recursos.*

## <a name="impact"></a>**Impacto**
*Para o tipo de pool de armazenamento especificado, os seguintes pools pai e filho compartilham o mesmo caminho de armazenamento:*

\<list of pools>

## <a name="resolution"></a>**Resolução**
*Use o Windows PowerShell para reconfigurar os pools de recursos de armazenamento para que vários pools não usem o mesmo caminho de armazenamento.*



