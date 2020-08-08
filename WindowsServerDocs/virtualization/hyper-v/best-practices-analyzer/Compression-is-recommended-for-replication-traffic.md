---
title: A compactação é recomendada para o tráfego de replicação
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: cf8be6e9-2909-4e4a-bb63-d1e1ebbc6930
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 2184a2ec0441af7fdbfc7566d783bee82f6bbbb9
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87954523"
---
# <a name="compression-is-recommended-for-replication-traffic"></a>A compactação é recomendada para o tráfego de replicação

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
*O tráfego de replicação enviado pela rede do servidor primário para o servidor de réplica é descompactado.*

## <a name="impact"></a>Impacto
*O tráfego de replicação usará mais largura de banda do que o necessário. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Configure a réplica do Hyper-V para compactar os dados transmitidos pela rede nas configurações da máquina virtual no Gerenciador do Hyper-V. Você também pode usar ferramentas fora do Hyper-V para executar a compactação.*



