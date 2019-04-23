---
title: Defina as configurações de TCP/IP de Failover que você deseja que a máquina virtual de réplica para usar no caso de um failover
description: Versão online do texto para essa regra do analisador de práticas recomendadas.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: KBDAzure
ms.date: 8/16/2016
ms.openlocfilehash: c16fbc95c9d679611d57327992a6621d58d4e201
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855747"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>Defina as configurações de TCP/IP de Failover que você deseja que a máquina virtual de réplica para usar no caso de um failover

>Aplica-se a: Windows Server 2016
 
Para obter mais informações sobre as práticas recomendadas e varreduras, consulte [Run Best Practices Analyzer Scans e Manage Scan Results](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Recurso do produto**|Hyper-V|  
|**Severidade**|Aviso|  
|**categoria**|Configuração|  
  
Nas seções a seguir, itálico indica o texto de interface do usuário que aparece na ferramenta Analisador de práticas recomendadas para esse problema.
  
## <a name="issue"></a>Problema  
*Máquinas virtuais de réplica configuradas com um endereço IP estático deve ser configuradas para usar um endereço IP diferente de sua contraparte de máquina virtual primária no caso de failover.*  
  
## <a name="impact"></a>Impacto  
*Os clientes que usam a carga de trabalho compatível com a máquina virtual primária podem não ser capazes de se conectar à máquina virtual de réplica após um failover. Além disso, endereço IP da máquina virtual primária original não serão válido na topologia da rede de máquina virtual de réplica. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para configurar o endereço IP que a máquina virtual de réplica deve usar no caso de failover.*  
  


