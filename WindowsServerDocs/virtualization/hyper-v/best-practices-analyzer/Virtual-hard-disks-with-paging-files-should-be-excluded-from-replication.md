---
title: Os discos rígidos virtuais com arquivos de paginação devem ser excluídos da replicação
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 215e265d69af1b384d3461c627558ff0a59c8e91
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364547"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Os discos rígidos virtuais com arquivos de paginação devem ser excluídos da replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Informações|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Os arquivos de paginação devem ser excluídos da participação na replicação, mas nenhum disco foi excluído.*  
  
## <a name="impact"></a>Impacto  
os arquivos *Paging apresentam um alto volume de atividade de entrada/saída, o que exigirá, desnecessariamente, recursos muito maiores para participar da replicação. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*If você ainda não tiver feito isso, crie um disco rígido virtual separado para o arquivo de paginação do Windows. Se a replicação inicial já tiver sido concluída, use o Gerenciador do Hyper-V para remover a replicação. Em seguida, configure a replicação novamente e exclua o disco rígido virtual com o arquivo de paginação da replicação.*  
  


