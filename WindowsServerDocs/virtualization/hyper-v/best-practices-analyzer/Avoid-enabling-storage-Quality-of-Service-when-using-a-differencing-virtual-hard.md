---
title: Evite habilitar a qualidade de serviço de armazenamento ao usar um disco rígido virtual diferencial quando o pai e filho discos rígidos virtuais estão em volumes diferentes
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: aa9ed408-65cf-40dc-aad2-118b54c70179
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 2bdc8462c4d9dc50dbb69792f2f294add0ca3a74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59856197"
---
# <a name="avoid-enabling-storage-quality-of-service-when-using-a-differencing-virtual-hard-disk-when-the-parent-and-child-virtual-hard-disks-are-on-different-volumes"></a>Evite habilitar a qualidade de serviço de armazenamento ao usar um disco rígido virtual diferencial quando o pai e filho discos rígidos virtuais estão em volumes diferentes

>Aplica-se a: Windows Server 2016

Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.
  
## <a name="issue"></a>**Problema**  
*Um disco de rígido virtual diferencial com os pai e filho discos rígidos virtuais em diferentes volumes tem armazenamento qualidade de serviço habilitados.*  
  
## <a name="impact"></a>**Impacto**  
*Essa configuração pode resultar em armazenamento inesperado comportamento de qualidade de serviço para o disco rígido virtual diferencial, bem como outros discos rígidos virtuais nos volumes de pai e filho. Isso afeta a seguir os discos rígidos virtuais:*  
  
\<lista de discos rígidos virtuais >  
  
## <a name="resolution"></a>**Resolução**  
*Desabilitar o armazenamento de qualidade de serviço nos discos rígidos virtuais referenciados ou executar uma migração de armazenamento para mover o pai e o disco rígido virtual filho para o mesmo volume.*  
  


