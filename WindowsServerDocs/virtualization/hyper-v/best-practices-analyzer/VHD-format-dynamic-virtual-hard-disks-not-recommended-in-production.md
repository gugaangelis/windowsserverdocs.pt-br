---
title: Discos rígidos virtuais dinâmicos de formato VHD não são recomendados para máquinas virtuais que executam cargas de trabalho de servidor em um ambiente de produção
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 324a60a0-1d15-4ef2-9f17-23cbd2eb42ce
ms.date: 8/16/2016
ms.openlocfilehash: 080858d709592fd877fc643219a7cca1bd0607ff
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746771"
---
# <a name="vhd-format-dynamic-virtual-hard-disks-are-not-recommended-for-virtual-machines-that-run-server-workloads-in-a-production-environment"></a>Discos rígidos virtuais dinâmicos de formato VHD não são recomendados para máquinas virtuais que executam cargas de trabalho de servidor em um ambiente de produção

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
*Uma ou mais máquinas virtuais usam discos rígidos virtuais de expansão dinâmica em formato VHD.*

## <a name="impact"></a>**Impacto**
*Os discos rígidos virtuais dinâmicos de formato VHD podem apresentar problemas de consistência se ocorrer uma falha de energia. Problemas de consistência podem ocorrer se o disco físico executar uma atualização incompleta ou incorreta para um setor em um arquivo. vhd que está sendo modificado quando ocorrer uma falha de energia. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>**Resolução**
*Desligue a máquina virtual e converta o disco rígido virtual dinâmico do formato VHD em um disco rígido virtual em formato VHDX ou em um disco rígido virtual fixo. (O formato VHDX tem mecanismos de confiabilidade que ajudam a proteger o disco contra corrupções devido a falhas de energia do sistema.) No entanto, não converta o disco rígido virtual se for provável que ele esteja anexado a uma versão anterior do Windows em algum momento. As versões do Windows anteriores ao Windows Server 2012 não dão suporte ao formato VHDX.*



