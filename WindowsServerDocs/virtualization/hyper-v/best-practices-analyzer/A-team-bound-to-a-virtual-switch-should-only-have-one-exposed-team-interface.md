---
title: Uma equipe associada a um comutador virtual deve ter apenas uma interface de equipe exposta
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 1074f086-1a2e-42e1-b58c-f55e657d5ce1
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 7ec4c25a86bf90f1b2416e0d53ded8f5319960ad
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87968513"
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



