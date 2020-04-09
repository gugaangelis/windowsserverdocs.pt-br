---
title: Evite configurar máquinas virtuais para permitir comandos SCSI não filtrados
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: dd4a3d78-a77f-451e-a383-d5cf45ea17cf
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: ac059bce1704a4e72b2c373d8186dbd4e31f2164
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857789"
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
  


