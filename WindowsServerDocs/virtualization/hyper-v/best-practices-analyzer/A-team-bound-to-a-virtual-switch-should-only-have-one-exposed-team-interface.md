---
title: Uma equipe associada a um comutador virtual deve ter apenas uma interface de equipe exposta
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
ms.date: 8/16/2016
ms.openlocfilehash: e600efe56c68f59ed8587e78a1d82576ff0c5c85
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746361"
---
# <a name="a-team-bound-to-a-virtual-switch-should-only-have-one-exposed-team-interface"></a>Uma equipe associada a um comutador virtual deve ter apenas uma interface de equipe exposta

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
*Um ou mais comutadores virtuais estão associados a uma equipe que tem várias interfaces de equipe.*

## <a name="impact"></a>Impacto
*Os seguintes comutadores virtuais podem não ter acesso a VLANs e largura de banda usada por outras interfaces de equipe:*

\<list of virtual switches>

## <a name="resolution"></a>Resolução
*Use o cmdlet do Windows PowerShell remove-NetLbfoTeamNic para remover todas as interfaces de equipe da equipe que não seja a interface de equipe padrão.*



