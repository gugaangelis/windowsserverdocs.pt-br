---
title: Configurar controladores SCSI somente quando houver suporte pelo sistema operacional convidado
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 861f194f-467e-4b07-a1c5-55b35f6327c4
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: da8d929a8f06f58610913d28d2f1e90299efb235
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71366422"
---
# <a name="configure-scsi-controllers-only-when-supported-by-the-guest-operating-system"></a>Configurar controladores SCSI somente quando houver suporte pelo sistema operacional convidado

>Aplica-se a: Windows Server 2016


  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categorias**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Uma máquina virtual está configurada com um controlador SCSI que não pode ser usado porque o sistema operacional convidado não oferece suporte a controladores SCSI.*  
  
## <a name="impact"></a>Impacto  
  
os computadores *Virtual não podem usar o armazenamento anexado ao controlador SCSI. Isso afeta as seguintes máquinas virtuais:*  
  
\<list de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
  
*Shut a máquina virtual e use o Gerenciador do Hyper-V para remover o controlador SCSI da máquina virtual. Em seguida, reinicie a máquina virtual.*  
  


