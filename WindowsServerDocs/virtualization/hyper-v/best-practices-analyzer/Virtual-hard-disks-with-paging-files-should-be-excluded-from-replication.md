---
title: Discos rígidos virtuais com arquivos de paginação devem ser excluídos da replicação
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: c0be8a5f-64a1-488a-944e-bb913bb90517
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 8b6289a82c83f3dcfc0de299250ce19ee3782678
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850117"
---
# <a name="virtual-hard-disks-with-paging-files-should-be-excluded-from-replication"></a>Discos rígidos virtuais com arquivos de paginação devem ser excluídos da replicação

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Informações do|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
*Arquivos de paginação devem ser excluídos da participação na replicação, mas nenhum disco foram excluído.*  
  
## <a name="impact"></a>Impacto  
*Arquivos de paginação experimentem um alto volume de atividade de entrada/saída, o que exigirá desnecessariamente quantidade maior de recursos para participar da replicação. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Se você ainda não fez isso, crie um disco de rígido virtual separado para o arquivo de paginação do Windows. Se já tiver sido concluída a replicação inicial, use o Gerenciador do Hyper-V para remover a replicação. Em seguida, configurar a replicação novamente e excluir o disco rígido virtual com o arquivo de paginação da replicação.*  
  


