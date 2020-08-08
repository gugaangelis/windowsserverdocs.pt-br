---
title: Usar todas as funções virtuais para rede quando elas estiverem disponíveis
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: bf895484-6a0d-4aa4-9a42-9fac739e875d
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 6be1d32b22d5619ff40d0121bcc60a6bfa3c225f
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960299"
---
# <a name="use-all-virtual-functions-for-networking-when-they-are-available"></a>Usar todas as funções virtuais para rede quando elas estiverem disponíveis

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
*Alguns recursos de aceleração de hardware não estão sendo utilizados*

## <a name="impact"></a>Impacto
*Essa configuração pode fazer com que a utilização geral da CPU seja maior do que o necessário. O desempenho de rede pode não ser o ideal nas seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Considere configurar o adaptador de rede virtual para SR-IOV se o hardware físico oferecer suporte a SR-IOV e se essa configuração não entrar em conflito com os recursos de rede exigidos pela máquina virtual.*



