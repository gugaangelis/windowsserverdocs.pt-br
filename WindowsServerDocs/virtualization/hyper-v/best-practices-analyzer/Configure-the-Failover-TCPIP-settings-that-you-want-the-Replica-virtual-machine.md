---
title: Definir as configurações de TCP/IP de failover que você deseja que a máquina virtual de réplica use no caso de um failover
description: Versão online do texto para esta regra de Analisador de Práticas Recomendadas.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: d2db5fdedbe2f19c01b7dd172f18b6fec969e828
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80862049"
---
# <a name="configure-the-failover-tcpip-settings-that-you-want-the-replica-virtual-machine-to-use-in-the-event-of-a-failover"></a>Definir as configurações de TCP/IP de failover que você deseja que a máquina virtual de réplica use no caso de um failover

>Aplica-se a: Windows Server 2016
 
Para obter mais informações sobre práticas recomendadas e verificações, consulte [executar verificações de analisador de práticas recomendadas e gerenciar resultados de verificação](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Propriedade|Detalhes|  
|-|-|  
|**Sistema Operacional**|Windows Server 2016|  
|**Produto/recurso**|Hyper-V|  
|**Severity**|Aviso|  
|**Categoria**|Configuração|  
  
Nas seções a seguir, os itálicos indicam o texto da interface do usuário que aparece na ferramenta de Analisador de Práticas Recomendadas para esse problema.
  
## <a name="issue"></a>Problema  
*As máquinas virtuais de réplica configuradas com um endereço IP estático devem ser configuradas para usar um endereço IP diferente de seu equivalente de máquina virtual primária no caso de failover.*  
  
## <a name="impact"></a>Impacto  
*Os clientes que usam a carga de trabalho com suporte da máquina virtual primária podem não conseguir se conectar à máquina virtual de réplica após um failover. Além disso, o endereço IP original da máquina virtual primária não será válido na topologia de rede da máquina virtual de réplica. Isso afeta as seguintes máquinas virtuais:*  
  
\<lista de máquinas virtuais >  
  
## <a name="resolution"></a>Resolução  
*Use o Gerenciador do Hyper-V para configurar o endereço IP que a máquina virtual de réplica deve usar no caso de failover.*  
  


