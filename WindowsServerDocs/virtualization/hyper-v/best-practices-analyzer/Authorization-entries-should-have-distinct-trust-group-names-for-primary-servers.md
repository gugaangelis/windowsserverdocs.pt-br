---
title: As entradas de autorização devem ter nomes de grupos de confiança distintos para servidores primários com máquinas virtuais que não fazem parte do mesmo grupo de confiança
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 8827a3a7-9f3c-4f51-826a-8e2ec43e01df
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 53cb81e0071dee09feffb6e37f2ab6dbece27d28
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87970003"
---
# <a name="authorization-entries-should-have-distinct-trust-group-names-for-primary-servers-with-virtual-machines-that-are-not-part-of-the-same-trust-group"></a>As entradas de autorização devem ter nomes de grupos de confiança distintos para servidores primários com máquinas virtuais que não fazem parte do mesmo grupo de confiança

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
*O servidor aceitará solicitações de replicação para a máquina virtual de réplica de qualquer um dos servidores na lista de autorização associada à mesma marca de replicação que a máquina virtual.*

## <a name="impact"></a>**Impacto**
*Pode haver preocupações de privacidade e segurança com uma máquina virtual que aceita a replicação de servidores primários que pertencem a diferentes entradas de autorização. Isso afeta as seguintes entradas de autorização:\<list of authorization entries>*

## <a name="resolution"></a>**Resolução**
*Use marcas diferentes nas entradas de autorização para servidores primários com máquinas virtuais que não fazem parte do mesmo grupo de segurança. Modifique as configurações do Hyper-V para configurar as marcas de replicação.*



