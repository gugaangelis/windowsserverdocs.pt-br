---
title: Os discos rígidos virtuais com arquivos de paginação devem ser excluídos da replicação
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.author: benarm
author: BenjaminArmstrong
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
ms.date: 8/16/2016
ms.openlocfilehash: 14729113ee2ba3694bcc29d50da5e7113c763268
ms.sourcegitcommit: dd1fbb5d7e71ba8cd1b5bfaf38e3123bca115572
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/17/2020
ms.locfileid: "90746561"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Os discos rígidos virtuais com arquivos de paginação devem ser excluídos da replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Informações|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*Os arquivos de paginação devem ser excluídos da participação na replicação, mas nenhum disco foi excluído.*

## <a name="impact"></a>Impacto
*Os arquivos de paginação experimentam um alto volume de atividade de entrada/saída, o que exigirá, desnecessariamente, recursos muito maiores para participar da replicação. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Se você ainda não tiver feito isso, crie um disco rígido virtual separado para o arquivo de paginação do Windows. Se a replicação inicial já tiver sido concluída, use o Gerenciador do Hyper-V para remover a replicação. Em seguida, configure a replicação novamente e exclua o disco rígido virtual com o arquivo de paginação da replicação.*



