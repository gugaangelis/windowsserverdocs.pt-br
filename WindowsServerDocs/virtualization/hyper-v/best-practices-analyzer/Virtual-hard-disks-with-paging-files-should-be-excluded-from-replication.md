---
title: Os discos rígidos virtuais com arquivos de paginação devem ser excluídos da replicação
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
manager: dongill
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 8f5c0cffa986e658d1ca750c11a6204bf8780a32
ms.sourcegitcommit: dfa48f77b751dbc34409aced628eb2f17c912f08
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87960219"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Os discos rígidos virtuais com arquivos de paginação devem ser excluídos da replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e varreduras, confira [Executar varreduras do Analisador de Práticas Recomendadas e gerenciar os resultados](https://go.microsoft.com/fwlink/p/?LinkID=223177).

|Propriedade|Detalhes|
|-|-|
|**Sistema operacional**|Windows Server 2016|
|**Produto/Recurso**|Hyper-V|
|**Gravidade**|Informação|
|**Categoria**|Configuração|

Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.

## <a name="issue"></a>Problema
*Os arquivos de paginação devem ser excluídos da participação na replicação, mas nenhum disco foi excluído.*

## <a name="impact"></a>Impacto
*Os arquivos de paginação experimentam um alto volume de atividade de entrada/saída, o que exigirá, desnecessariamente, recursos muito maiores para participar da replicação. Isso afeta as seguintes máquinas virtuais:*

\<list of virtual machines>

## <a name="resolution"></a>Resolução
*Se você ainda não tiver feito isso, crie um disco rígido virtual separado para o arquivo de paginação do Windows. Se a replicação inicial já tiver sido concluída, use o Gerenciador do Hyper-V para remover a replicação. Em seguida, configure a replicação novamente e exclua o disco rígido virtual com o arquivo de paginação da replicação.*



