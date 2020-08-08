---
title: Máquinas virtuais configuradas com um adaptador de Fibre Channel virtual devem ser configuradas para alta disponibilidade para o armazenamento baseado em Fibre Channel
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 73127bdd-8086-4268-a93c-2fdf1623e91b
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 60a96d6e559f3fefe6f8c1c52c2d145efa006c99
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960179"
---
# <a name="virtual-machines-configured-with-a-virtual-fibre-channel-adapter-should-be-configured-for-high-availability-to-the-fibre-channel-based-storage"></a>Máquinas virtuais configuradas com um adaptador de Fibre Channel virtual devem ser configuradas para alta disponibilidade para o armazenamento baseado em Fibre Channel

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Informação|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>**Problema**
*Uma ou mais máquinas virtuais não têm uma conexão altamente disponível com o armazenamento baseado em Fibre Channel porque essas máquinas virtuais são configuradas com um adaptador de Fibre Channel virtual que está conectado a apenas um HBA (adaptador de barramento de host).*

## <a name="impact"></a>**Impacto**
*Uma falha do adaptador de barramento de host pode bloquear a conexão de Fibre Channel entre o armazenamento e as máquinas virtuais. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolução**
*Adicione outra conexão da máquina virtual ao adaptador de barramento de host e configure o MPIO (Multipath I/O) no sistema operacional convidado para estabelecer conexões de Fibre Channel redundantes.*



