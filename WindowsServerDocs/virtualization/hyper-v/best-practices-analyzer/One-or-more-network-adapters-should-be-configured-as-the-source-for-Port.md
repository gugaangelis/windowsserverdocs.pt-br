---
title: Um ou mais adaptadores de rede devem ser configurados como a origem do espelhamento de porta
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 147fd00f-1440-44d1-94e3-3a8af63aa7ed
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 1d5f355e4553d5d775152ed405729fa32c829acf
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954453"
---
# <a name="one-or-more-network-adapters-should-be-configured-as-the-source-for-port-mirroring"></a>Um ou mais adaptadores de rede devem ser configurados como a origem do espelhamento de porta

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
*Uma ou mais máquinas virtuais têm um adaptador de rede configurado como destino para o espelhamento de porta, mas não há nenhuma origem correspondente no comutador virtual.*

## <a name="impact"></a>**Impacto**
*O espelhamento de porta não funcionará corretamente para os seguintes comutadores virtuais e máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolução**
*Use o Windows PowerShell ou o Gerenciador do Hyper-V para concluir ou corrigir a configuração de espelhamento de porta.*



