---
title: Os discos rígidos virtuais com arquivos de paginação devem ser excluídos da replicação
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 94e03cf9de3991d003fad9019b9af33fad2f6bae
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855019"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Os discos rígidos virtuais com arquivos de paginação devem ser excluídos da replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|{1&gt;Informações&lt;1}|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Os arquivos de paginação devem ser excluídos da participação na replicação, mas nenhum disco foi excluído.*  
  
## <a name="impact"></a>Impacto  
*Os arquivos de paginação experimentam um alto volume de atividade de entrada/saída, o que exigirá, desnecessariamente, recursos muito maiores para participar da replicação. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Se você ainda não tiver feito isso, crie um disco rígido virtual separado para o arquivo de paginação do Windows. Se a replicação inicial já tiver sido concluída, use o Gerenciador do Hyper-V para remover a replicação. Em seguida, configure a replicação novamente e exclua o disco rígido virtual com o arquivo de paginação da replicação.*  
  


