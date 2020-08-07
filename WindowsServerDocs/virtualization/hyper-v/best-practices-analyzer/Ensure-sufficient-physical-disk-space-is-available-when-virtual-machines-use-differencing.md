---
title: Verifique se há espaço em disco físico suficiente disponível quando as máquinas virtuais usam discos rígidos virtuais diferenciais
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: 71f99aab-f994-4022-9da0-d661965b95ac
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 9915d01408ec5d51cc70bcdf4682ebca5b4e5c32
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87950280"
---
# <a name="ensure-sufficient-physical-disk-space-is-available-when-virtual-machines-use-differencing-virtual-hard-disks"></a>Verifique se há espaço em disco físico suficiente disponível quando as máquinas virtuais usam discos rígidos virtuais diferenciais

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
*Uma ou mais máquinas virtuais estão usando discos rígidos virtuais diferenciais.*

## <a name="impact"></a>Impacto
*Os discos rígidos virtuais diferenciais exigem o espaço disponível no volume de hospedagem para que o espaço possa ser alocado quando ocorrerem gravações nos discos rígidos virtuais. Se o espaço disponível for esgotado, qualquer máquina virtual que dependa do armazenamento físico poderá ser afetada. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Monitore o espaço em disco disponível para garantir que haja espaço suficiente disponível para a expansão do disco rígido virtual. Considere mesclar discos rígidos virtuais diferenciais em seu pai. No Gerenciador do Hyper-V, inspecione o disco diferencial para determinar o disco rígido virtual pai. Se você mesclar um disco diferencial a um disco pai que é compartilhado por outros discos diferenciais, essa ação corromperá a relação entre os outros discos diferenciais e o disco pai, tornando-os inutilizáveis. Depois de verificar se o disco rígido virtual pai não está compartilhado, você pode usar o assistente para editar disco para mesclar o disco diferencial para o disco rígido virtual pai.*



