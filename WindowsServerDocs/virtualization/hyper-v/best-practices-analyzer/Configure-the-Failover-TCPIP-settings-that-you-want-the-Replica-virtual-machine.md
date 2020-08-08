---
title: Definir as configurações de TCP/IP de failover que você deseja que a máquina virtual de réplica use no caso de um failover
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 713be5cf428617287e0be0bc65b3e2beb2d11400
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87948444"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>Definir as configurações de TCP/IP de failover que você deseja que a máquina virtual de réplica use no caso de um failover

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
*As máquinas virtuais de réplica configuradas com um endereço IP estático devem ser configuradas para usar um endereço IP diferente de seu equivalente de máquina virtual primária no caso de failover.*

## <a name="impact"></a>Impacto
*Os clientes que usam a carga de trabalho com suporte da máquina virtual primária podem não conseguir se conectar à máquina virtual de réplica após um failover. Além disso, o endereço IP original da máquina virtual primária não será válido na topologia de rede da máquina virtual de réplica. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Use o Gerenciador do Hyper-V para configurar o endereço IP que a máquina virtual de réplica deve usar no caso de failover.*



