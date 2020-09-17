---
title: A configuração do PVLAN em um comutador virtual deve ser consistente
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 4db63bcc-7a54-4f19-98a6-c274c3956d51
ms.date: 8/16/2016
ms.openlocfilehash: 205917e793bfb0cc398f7309981109083189d439
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90745581"
---
# <a name="pvlan-configuration-on-a-virtual-switch-must-be-consistent"></a>A configuração do PVLAN em um comutador virtual deve ser consistente

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Erro|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>**Problema**
*A rede de área local virtual (PVLAN) privada não está configurada corretamente em um ou mais adaptadores de rede virtual.*

## <a name="impact"></a>**Impacto**
*PVLAN pode não isolar o tráfego de rede entre máquinas virtuais corretamente.*

## <a name="resolution"></a>**Resolução**
*Use o cmdlet do Windows PowerShell, Set-VMNetworkAdapterVlan, para configurar o PVLAN corretamente.*



