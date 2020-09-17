---
title: Habilitar todos os adaptadores de rede virtual configurados para uma máquina virtual
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: fcd350b7-4240-4359-aadd-93e7ac4d314e
ms.date: 8/16/2016
ms.openlocfilehash: 81e0bbb8fce32b8343a13dea953498efb627d496
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746281"
---
# <a name="enable-all-virtual-network-adapters-configured-for-a-virtual-machine"></a>Habilitar todos os adaptadores de rede virtual configurados para uma máquina virtual

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, consulte [Analisador de Práticas Recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Aviso|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema

*Um ou mais adaptadores de rede podem estar desabilitados em uma máquina virtual.*

## <a name="impact"></a>Impacto

*As seguintes máquinas virtuais podem não ter conectividade de rede:*

\<list of virtual machine names>

## <a name="resolution"></a>Resolução

*Use Device Manager no sistema operacional convidado para habilitar todos os adaptadores de rede virtual. Se o adaptador não for necessário, use o Gerenciador do Hyper-V para removê-lo da máquina virtual.*



