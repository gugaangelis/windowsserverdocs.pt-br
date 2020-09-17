---
title: Evite as inconsistências de alinhamento entre blocos virtuais e setores de disco físico em discos rígidos virtuais dinâmicos ou discos diferenciais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: a17c8fd2-af81-485b-bfea-bd1ef3e43923
ms.date: 8/16/2016
ms.openlocfilehash: aa165dad38ae455f7c4a5a0d73cdd005c01a60ef
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90747091"
---
# <a name="avoid-alignment-inconsistencies-between-virtual-blocks-and-physical-disk-sectors-on-dynamic-virtual-hard-disks-or-differencing-disks"></a>Evite as inconsistências de alinhamento entre blocos virtuais e setores de disco físico em discos rígidos virtuais dinâmicos ou discos diferenciais

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
*Foram detectadas inconsistências de alinhamento para um ou mais discos rígidos virtuais.*

### <a name="impact"></a>Impacto
*Se os discos rígidos virtuais estiverem armazenados no disco físico que tem um tamanho de setor de 4K, a máquina virtual ou os aplicativos que usam o disco rígido virtual poderão apresentar problemas de desempenho. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Use o assistente para criar disco rígido virtual para criar um novo disco rígido virtual em formato VHD ou VHDX e especifique o disco rígido virtual existente como o disco de origem. O novo disco rígido virtual será criado com o alinhamento entre os blocos virtuais e o disco físico.*



