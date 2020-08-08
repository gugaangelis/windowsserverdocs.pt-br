---
title: Verifique se o driver de função virtual opera corretamente quando uma máquina virtual está configurada para usar SR-IOV
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: d21e4b93-29bf-423a-a635-71c6d48dc49e
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 8fc14e14ff66b59099ebad1f3b9643b94427346a
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954483"
---
# <a name="ensure-that-the-virtual-function-driver-operates-correctly-when-a-virtual-machine-is-configured-to-use-sr-iov"></a>Verifique se o driver de função virtual opera corretamente quando uma máquina virtual está configurada para usar SR-IOV

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
*O driver de função virtual não está operando corretamente no sistema operacional convidado de uma ou mais máquinas virtuais.*

## <a name="impact"></a>Impacto
*O desempenho de rede não é o ideal nas seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*No sistema operacional convidado, faça o seguinte: Verifique se os drivers apropriados estão instalados e se todos os dispositivos de rede estão habilitados e verifique se há erros ou avisos no log de eventos.*



