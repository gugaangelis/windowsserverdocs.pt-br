---
title: Evite configurar máquinas virtuais para permitir que os comandos SCSI não filtrados
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: f401ce4d72f88d72529a95acea2a999df93679b6
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888267"
---
# <a name="avoid-configuring-virtual-machines-to-allow-unfiltered-scsi-commands"></a>Evite configurar máquinas virtuais para permitir que os comandos SCSI não filtrados

>Aplica-se a: Windows Server 2016


  
*Para obter mais informações sobre as práticas recomendadas e varreduras, consulte* [Best Practices Analyzer](https://go.microsoft.com/fwlink/?LinkId=122786).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Operações|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.  
  
## <a name="issue"></a>Problema  
  
*Uma máquina virtual é configurada para permitir que os comandos SCSI não filtrados.*  
  
## <a name="impact"></a>Impacto  
  
*Ignorando o comando SCSI filtragem apresenta um risco à segurança. Essa configuração deve ser habilitada apenas se for necessário para compatibilidade com aplicativos de armazenamento em execução no sistema operacional convidado. As seguintes máquinas virtuais são configuradas para permitir os comandos SCSI não filtrados:*  
  
\<lista de nomes de máquina virtual >  
  
## <a name="resolution"></a>Resolução  
  
*Entre em contato com seu fornecedor de armazenamento para determinar se essa configuração é necessária. Além disso, se o sistema operacional de gerenciamento ou outros sistemas operacionais convidados sejam comprometidos ou apresentam comportamento incomum, reconfigure a máquina virtual para bloquear os comandos.*  
  


