---
title: Os adaptadores de vídeo devem ser habilitados em máquinas virtuais para fornecer recursos de vídeo
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: ac5992e6-3c0b-46c2-a48e-6ef37b679228
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ba22545a0e1d66a0f0d8f03dd7f7a42054a41d44
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960609"
---
# <a name="display-adapters-should-be-enabled-in-virtual-machines-to-provide-video-capabilities"></a>Os adaptadores de vídeo devem ser habilitados em máquinas virtuais para fornecer recursos de vídeo

>Aplica-se a: Windows Server 2016



*Para obter mais informações sobre práticas recomendadas e verificações, consulte* [analisador de práticas recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*O dispositivo de vídeo do barramento de máquina virtual da Microsoft pode estar desabilitado em uma máquina virtual.*

O dispositivo de vídeo do barramento de máquina virtual da Microsoft é um adaptador de vídeo virtual otimizado para uso com máquinas virtuais do Hyper-V. Quando uma máquina virtual não está configurada para usar o dispositivo de vídeo do barramento de máquina virtual da Microsoft, é usado um adaptador de vídeo herdado. O dispositivo de vídeo do barramento de máquina virtual da Microsoft executa melhor do que um adaptador de vídeo herdado.

## <a name="impact"></a>Impacto

*O desempenho de vídeo para as seguintes máquinas virtuais será degradado:*

\<list of virtual machine names>

## <a name="resolution"></a>Resolução

*Use Device Manager no sistema operacional convidado para habilitar o dispositivo de vídeo do barramento de máquina virtual da Microsoft.*

As etapas necessárias para usar Device Manager variam de acordo com o sistema operacional. Para obter instruções, consulte a ajuda no sistema operacional convidado.



