---
title: Evite configurar máquinas virtuais para permitir comandos SCSI não filtrados
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 5deb20862ed0e359febd4a9b58202d53c85058ca
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/27/2019
ms.locfileid: "71365274"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>Evite configurar máquinas virtuais para permitir comandos SCSI não filtrados

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre práticas recomendadas e verificações, consulte* [analisador de práticas recomendadas](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Operações|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Uma máquina virtual está configurada para permitir comandos SCSI não filtrados.*  
  
## <a name="impact"></a>Impacto  
  
*Ignorar a filtragem de comando SCSI representa um risco de segurança. Essa configuração deve ser habilitada somente se for necessária para compatibilidade com aplicativos de armazenamento em execução no sistema operacional convidado. As seguintes máquinas virtuais estão configuradas para permitir comandos SCSI não filtrados:*  
  
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Entre em contato com seu fornecedor de armazenamento para determinar se essa configuração é necessária. Além disso, se o sistema operacional de gerenciamento ou outros sistemas operacionais convidados estiverem comprometidos ou apresentarem comportamento incomum, reconfigure a máquina virtual para bloquear os comandos.*  
  


