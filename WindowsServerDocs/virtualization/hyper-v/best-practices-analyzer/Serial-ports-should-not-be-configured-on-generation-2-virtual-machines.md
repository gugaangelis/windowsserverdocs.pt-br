---
title: As portas seriais não devem ser configuradas em máquinas virtuais de geração 2
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 87061193-dd3f-4398-aa5d-4cee83cadfa3
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: b842adcb098a76edc6c42020dd87f8f3e224d9bd
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87938975"
---
# <a name="serial-ports-should-not-be-configured-on-generation-2-virtual-machines"></a>As portas seriais não devem ser configuradas em máquinas virtuais de geração 2

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
*Uma ou mais máquinas virtuais de geração 2 têm uma porta serial configurada.*

## <a name="impact"></a>**Impacto**
*O desempenho pode ser afetado pelas seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolução**
*Se isso for intencional, nenhuma ação adicional será necessária. Caso contrário, considere o uso do Gerenciador do Hyper-V ou do Windows PowerShell para remover a cadeia de conexão das portas seriais na máquina virtual.*



