---
title: Uma rede SAN virtual deve ser associada um adaptador de barramento do host físico
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 14bca69b-e779-4e90-b5c1-1b015625572f
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: 3b9ca1e2da1cf9f4410f465fe95c6cc9c0b07ffc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819077"
---
# <a name="a-virtual-san-should-be-associated-with-a-physical-host-bus-adapter"></a>Uma rede SAN virtual deve ser associada um adaptador de barramento do host físico

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
*Uma rede virtual de armazenamento (SAN) tiver sido configurada sem uma associação a um adaptador de barramento do host (HBA).*  
  
## <a name="impact"></a>**Impacto**  
*Uma máquina virtual não iniciará quando ela é configurada com um adaptador de Fibre Channel virtual conectado a uma SAN virtual configurado incorretamente. Isso afeta a SANs virtuais a seguir:*  
  
  
\<lista de SANs virtuais >  
  
  
## <a name="resolution"></a>**Resolução**  
*Reconfigure a rede SAN virtual conectando-a um adaptador de barramento do host.*  
  
  
  


