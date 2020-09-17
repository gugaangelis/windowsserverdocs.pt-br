---
title: Garantir que o espaço em disco físico suficiente esteja disponível quando as máquinas virtuais usam discos rígidos virtuais de expansão dinâmica
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: 9e3e3e64-4b3a-4b9d-acf1-e4df61a04f1e
ms.date: 8/16/2016
ms.openlocfilehash: 1e67b51aa3c41c9642a8972d25e0501b26e91cd5
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746861"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-dynamically-expanding-virtual-hard-disks"></a>Garantir que o espaço em disco físico suficiente esteja disponível quando as máquinas virtuais usam discos rígidos virtuais de expansão dinâmica

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
*Uma ou mais máquinas virtuais estão usando discos rígidos virtuais de expansão dinâmica.*

## <a name="impact"></a>Impacto
*A expansão dinâmica de discos rígidos virtuais requer espaço disponível no volume de hospedagem para que o espaço possa ser alocado quando as gravações nos discos rígidos virtuais ocorrerem. Se o espaço disponível for esgotado, qualquer máquina virtual que dependa do armazenamento físico poderá ser afetada. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Monitore o espaço em disco disponível para garantir que haja espaço suficiente disponível para expansão. Considere desligar a máquina virtual e usar o assistente para editar disco no Gerenciador do Hyper-V para converter cada disco rígido virtual de expansão dinâmica dessa máquina virtual em um disco rígido virtual de tamanho fixo.*



